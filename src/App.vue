<template>
  <div class="app-container">
    <!-- 顶部生成成功浮动 Toast -->
    <transition name="fade">
      <div v-if="showSuccessToast" class="top-success-toast">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
          <polyline points="20 6 9 17 4 12"></polyline>
        </svg>
        <span>创意表情包图片生成成功！</span>
      </div>
    </transition>

    <!-- 右上角常驻分享按钮 -->
    <button class="floating-share-btn" @click="showShareGuide = true">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="share-icon">
        <circle cx="18" cy="5" r="3"></circle>
        <circle cx="6" cy="12" r="3"></circle>
        <circle cx="18" cy="19" r="3"></circle>
        <line x1="8.59" y1="13.51" x2="15.42" y2="17.49"></line>
        <line x1="15.41" y1="6.51" x2="8.59" y2="10.49"></line>
      </svg>
      <span>分享</span>
    </button>

    <!-- 顶部 App Header (已移除未登录额度提示栏) -->
    <header>
      <h1>{{ appTitle }}</h1>
      <p>智能 AI 体验引擎 · 自定义主角与四种特色画风生图</p>
    </header>

    <!-- 活跃动态与使用人数轮播 -->
    <UserTicker />

    <!-- 核心输入与生成卡片 (模块一：一键体验) -->
    <main class="glass-card input-group">
      <div class="selector-group">
        <label class="selector-label">输入表情包文字或搞笑文案</label>
        <textarea 
          v-model="userInput" 
          placeholder="比如：又到了打工人最喜欢的星期五！发射火箭去火星开会，有事留言..."
          rows="4"
        ></textarea>
      </div>

      <div class="selector-group">
        <label class="selector-label">选择表情包主角角色</label>
        <div class="role-selector">
          <button 
            v-for="role in roleOptions" 
            :key="role.value"
            class="role-option"
            :class="{ active: activeRole === role.value }"
            @click="activeRole = role.value"
          >
            {{ role.label }}
          </button>
        </div>
      </div>

      <div class="selector-group">
        <label class="selector-label">选择表情包画面画风</label>
        <div class="style-selector">
          <button 
            v-for="style in styleOptions" 
            :key="style.value"
            class="style-option"
            :class="{ active: activeStyle === style.value }"
            @click="activeStyle = style.value"
          >
            {{ style.label }}
          </button>
        </div>
      </div>

      <button 
        class="action-btn" 
        :disabled="loading || !userInput.trim()"
        @click="handleGenerate"
      >
        {{ loading ? '正在由 AI 大模型绘制表情包中...' : '开始一键生成表情包' }}
      </button>

      <!-- 异常提示 -->
      <div v-if="errorMsg" class="error-banner">
        {{ errorMsg }}
      </div>
    </main>

    <!-- 生成结果卡片 (模块二：内容产出与分享) -->
    <section v-if="result || loading" class="glass-card result-section">
      <div class="result-header">
        <div class="result-title-group">
          <span class="result-title">生成结果</span>
          <span v-if="result" class="success-badge">生成成功</span>
        </div>
        <div class="button-actions">
          <button v-if="result && !isImageProject" class="icon-btn" @click="copyText">
            {{ copied ? '已复制' : '复制文案' }}
          </button>
          <button v-if="result" class="icon-btn highlight" @click="showShareCard = true">
            生成分享卡片
          </button>
          <a v-if="result && isImageProject" :href="result" target="_blank" download="ai-meme.png" class="icon-btn" style="text-decoration: none;">
            查看原图
          </a>
        </div>
      </div>

      <!-- 加载中骨架屏 -->
      <div v-if="loading" class="skeleton">
        <div class="skeleton-line" style="width: 80%"></div>
        <div class="skeleton-line" style="width: 95%"></div>
        <div class="skeleton-line" style="width: 60%"></div>
        <div class="skeleton-line" style="width: 75%"></div>
      </div>

      <!-- 渲染结果 -->
      <div v-else-if="result">
        <img v-if="isImageProject" :src="result" alt="Generated visual" class="image-output" />
        <div v-else class="output-content">{{ result }}</div>
      </div>
    </section>

    <!-- 演示案例区组件 (模块三：30 条表情包精选案例展示) -->
    <DemoShowcase @use-sample="handleUseSample" />

    <!-- 底部隐私与服务条款链接 -->
    <footer class="footer-links">
      <button class="footer-link-btn" @click="showPrivacy = true">Privacy Policy</button>
      <button class="footer-link-btn" @click="showTerms = true">Terms of Service</button>
      <button class="footer-link-btn" @click="showContact = true">Contact Us</button>
    </footer>

    <!-- 隐私政策弹窗 -->
    <div v-if="showPrivacy" class="modal-overlay" @click.self="showPrivacy = false">
      <div class="modal-content">
        <h3>Privacy Policy</h3>
        <div class="modal-text-content modal-scroll-area">
          <p>我们非常重视您的隐私。您在本应用中输入的配文与角色选择仅用于实时大模型生图，我们不会在服务器端进行永久存储或记录。</p>
          <p>为了记录您的免费额度，本应用会在您的浏览器本地（localStorage）记录试用次数与解锁状态。</p>
        </div>
        <button class="modal-btn" @click="showPrivacy = false">关闭</button>
      </div>
    </div>

    <!-- 服务条款弹窗 -->
    <div v-if="showTerms" class="modal-overlay" @click.self="showTerms = false">
      <div class="modal-content">
        <h3>Terms of Service</h3>
        <div class="modal-text-content modal-scroll-area">
          <p>欢迎使用我们的 AI 表情包生成器微应用。本应用仅用于创意娱乐、社交玩梗与插画设计展示。</p>
          <p>请确保输入的配文符合法律法规与社会公序良俗，生成图片版权遵循大模型平台协议。</p>
        </div>
        <button class="modal-btn" @click="showTerms = false">关闭</button>
      </div>
    </div>

    <!-- 联系我们弹窗 (自适应高度 + weixin.png & dingtalk.png 展示) -->
    <div v-if="showContact" class="modal-overlay" @click.self="showContact = false">
      <div class="modal-content contact-modal-content">
        <h3>Contact Us</h3>
        <div class="modal-text-content contact-card-body">
          <p>如果您在使用过程中遇到任何问题，或有合作意向，可以通过以下方式联系我们：</p>
          <div class="contact-qr-container">
            <div class="contact-qr-card">
              <img :src="weixinImg" alt="微信联系" class="contact-qr-img" />
              <span class="contact-qr-label">微信联系</span>
            </div>
            <div class="contact-qr-card">
              <img :src="dingtalkImg" alt="钉钉交流" class="contact-qr-img" />
              <span class="contact-qr-label">钉钉交流</span>
            </div>
          </div>
          <p class="contact-email">反馈邮箱: <span style="color: var(--primary-color);">us@wuxian.xyz</span></p>
        </div>
        <button class="modal-btn" @click="showContact = false">关闭</button>
      </div>
    </div>

    <!-- 裂变拦截弹窗 (模块四：裂变机制) -->
    <FissionModal 
      :visible="showFission" 
      :wechat-id="wechatId"
      @unlocked="handleUnlocked"
    />

    <!-- 分享卡片弹窗 (模块二扩展) -->
    <ShareCardModal
      :visible="showShareCard"
      :image-url="result"
      :caption="userInput"
      :wechat-id="wechatId"
      @close="showShareCard = false"
    />

    <!-- 微信 H5 分享引导浮层 -->
    <div v-if="showShareGuide" class="share-guide-overlay" @click="handleShareClose">
      <div class="share-guide-arrow">↗</div>
      <div class="share-guide-content">
        <p>点击右上角菜单 <strong>•••</strong></p>
        <p>选择 <strong>「分享到朋友圈」</strong></p>
        <p class="share-guide-sub">分享这款高效率的 AI 智能微应用</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import UserTicker from './components/UserTicker.vue';
import FissionModal from './components/FissionModal.vue';
import DemoShowcase from './components/DemoShowcase.vue';
import ShareCardModal from './components/ShareCardModal.vue';
import appConfig from './config.json';
import weixinImg from '../asset/weixin.png';
import dingtalkImg from '../asset/dingtalk.png';

const appTitle = ref(appConfig.title || '网腾无限AI 表情包生成器');
const wechatId = ref(appConfig.wechatId || 'ai_wuxian_xyz');

const userInput = ref('');
const loading = ref(false);
const errorMsg = ref('');
const result = ref('');
const copied = ref(false);
const showSuccessToast = ref(false);
const showFission = ref(false);
const showPrivacy = ref(false);
const showTerms = ref(false);
const showContact = ref(false);
const showShareGuide = ref(false);
const showShareCard = ref(false);

const handleShareClose = () => {
  showShareGuide.value = false;
  localStorage.setItem('shared_fission', 'true');
};

const getCookie = (name: string): string | null => {
  const nameEQ = name + "=";
  const ca = document.cookie.split(';');
  for (let i = 0; i < ca.length; i++) {
    let c = ca[i];
    while (c.charAt(0) === ' ') c = c.substring(1, c.length);
    if (c.indexOf(nameEQ) === 0) return c.substring(nameEQ.length, c.length);
  }
  return null;
};

const userToken = ref(getCookie('wuxian_session'));
const isLoggedIn = computed(() => !!userToken.value);
const authUsesCount = ref(parseInt(localStorage.getItem('auth_uses') || '0', 10));

const isImageProject = computed(() => {
  return appConfig.type === 'image' || true;
});

// 表情包角色选项
const roleOptions = [
  { label: '经典熊猫头', value: '经典熊猫头搞笑包子脸' },
  { label: '马斯克', value: '马斯克 (Elon Musk) 搞怪表情' },
  { label: '乔布斯', value: '乔布斯 (Steve Jobs) 极简风格' },
  { label: '比尔盖茨', value: '比尔盖茨 (Bill Gates) 怀旧风格' },
  { label: '扎克伯格', value: '扎克伯格 (Mark Zuckerberg) 赛博风格' },
  { label: '贝索斯', value: '贝索斯 (Jeff Bezos) 霸气快递风格' },
  { label: '柏拉图', value: '柏拉图 (Plato) 理想国雕像风格' },
  { label: '爱因斯坦', value: '爱因斯坦 (Einstein) 吐舌头疯狂科学家' },
  { label: '特斯拉', value: '尼古拉·特斯拉 (Tesla) 闪电朋克' },
  { label: '秦始皇', value: '秦始皇霸气帝王风格' },
];
const activeRole = ref(roleOptions[0].value);

// 表情包画面画风选项
const styleOptions = [
  { label: '经典黑白线条画', value: '经典黑白素描简笔线条画表情包' },
  { label: '写实搞怪大头贴', value: '夸张滑稽写实大头贴质感表情包' },
  { label: '3D粘土卡通', value: '色彩绚丽的3D粘土卡通公仔极具微距感' },
  { label: '复古8位像素', value: '8位复古红白机像素艺术画风' },
];
const activeStyle = ref(styleOptions[0].value);

const isLimitReached = computed(() => {
  if (isLoggedIn.value) {
    return authUsesCount.value >= 15;
  }
  const uses = parseInt(localStorage.getItem('free_uses') || '0', 10);
  const shared = localStorage.getItem('shared_fission') === 'true';
  return uses >= 3 && !shared;
});

const apiEndpoint = import.meta.env.DEV
  ? '/api/local/generate'
  : (import.meta.env.VITE_API_ENDPOINT || 'https://api.wuxian.xyz/api/v1/generate');

const triggerSuccessToast = () => {
  showSuccessToast.value = true;
  setTimeout(() => {
    showSuccessToast.value = false;
  }, 3000);
};

const handleGenerate = async () => {
  if (isLimitReached.value) {
    showFission.value = true;
    return;
  }

  loading.value = true;
  errorMsg.value = '';
  result.value = '';

  try {
    const fullPrompt = `请生成一张高清搞笑社交表情包。表情包主角角色：${activeRole.value}。表情包画面风格：${activeStyle.value}。表情包配文与画面主题：“${userInput.value}”。画面必须生动幽默，适合微信聊天梗图。`;

    const response = await fetch(apiEndpoint, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      credentials: 'include',
      body: JSON.stringify({
        taskType: 'image',
        prompt: fullPrompt,
        style: activeStyle.value
      })
    });

    const data = await response.json();
    if (data.error) {
      errorMsg.value = data.error;
    } else {
      result.value = data.result;
      triggerSuccessToast();
      
      if (isLoggedIn.value) {
        const nextAuthUses = authUsesCount.value + 1;
        localStorage.setItem('auth_uses', nextAuthUses.toString());
        authUsesCount.value = nextAuthUses;
      } else {
        const currentUses = parseInt(localStorage.getItem('free_uses') || '0', 10);
        localStorage.setItem('free_uses', (currentUses + 1).toString());
      }
    }
  } catch (err: any) {
    errorMsg.value = '生图接口失败，请检查网络或本地代理服务。';
  } finally {
    loading.value = false;
  }
};

const handleUseSample = (sampleCaption: string) => {
  userInput.value = sampleCaption;
  window.scrollTo({ top: 0, behavior: 'smooth' });
};

const handleUnlocked = () => {
  showFission.value = false;
  handleGenerate();
};

const copyText = async () => {
  try {
    await navigator.clipboard.writeText(userInput.value);
    copied.value = true;
    setTimeout(() => {
      copied.value = false;
    }, 2000);
  } catch (err) {
    errorMsg.value = '复制失败，请手动选择复制。';
  }
};
</script>
