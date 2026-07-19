<template>
  <section class="glass-card showcase-container">
    <div class="showcase-header">
      <div class="showcase-title-group">
        <h2 class="showcase-title">表情包创作演示案例库 (30 精选样例)</h2>
        <p class="showcase-subtitle">体验不同主角与四种画风的创意梗图，点击“一键同款生成”即可即刻创作</p>
      </div>
      <div class="showcase-badge">社交玩梗 · 免费体验</div>
    </div>

    <!-- 搜索与分类筛选 -->
    <div class="showcase-filter-bar">
      <div class="category-tabs">
        <button 
          v-for="cat in categories" 
          :key="cat"
          class="category-tab"
          :class="{ active: currentCategory === cat }"
          @click="currentCategory = cat"
        >
          {{ cat }}
        </button>
      </div>
      <div class="search-input-wrapper">
        <input 
          v-model="searchQuery"
          type="text"
          placeholder="搜索表情包文案、主角角色或画面风格..."
          class="search-input"
        />
      </div>
    </div>

    <!-- 样例列表格 Grid -->
    <div class="sample-grid">
      <div 
        v-for="sample in paginatedSamples" 
        :key="sample.id" 
        class="sample-card"
      >
        <div class="sample-card-header">
          <span class="topic-category-tag">{{ sample.category }}</span>
          <span class="role-tag">{{ sample.role }}</span>
        </div>
        <div class="sample-original">
          <span class="sample-label">表情包配文：</span>“{{ sample.caption }}”
        </div>
        <div class="sample-style-tag-line">
          <span class="sample-label">画面风格：</span><span class="style-name">{{ sample.style }}</span>
        </div>
        <div class="sample-rewritten">
          <span class="sample-label">画面设想：</span>{{ sample.scene }}
        </div>
        <div class="sample-card-footer">
          <button class="use-sample-btn" @click="$emit('use-sample', sample.caption)">
            一键同款生成
          </button>
        </div>
      </div>
    </div>

    <!-- 空状态提示 -->
    <div v-if="filteredSamples.length === 0" class="empty-showcase">
      未找到匹配的表情包样例，请尝试切换分类或重置搜索关键词。
    </div>

    <!-- 分页组件 -->
    <div v-if="filteredSamples.length > pageSize" class="pagination-bar">
      <button 
        class="page-btn" 
        :disabled="currentPage === 1"
        @click="currentPage--"
      >
        上一页
      </button>
      <span class="page-info">第 {{ currentPage }} / {{ totalPages }} 页 (共 {{ filteredSamples.length }} 条)</span>
      <button 
        class="page-btn" 
        :disabled="currentPage === totalPages"
        @click="currentPage++"
      >
        下一页
      </button>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, computed, watch } from 'vue';

defineEmits<{
  (e: 'use-sample', text: string): void;
}>();

const categories = ['全部', '职场玩梗', '科技巨头', '生活搞笑', '社恐解压'];
const currentCategory = ref('全部');
const searchQuery = ref('');
const currentPage = ref(1);
const pageSize = 6;

interface MemeSample {
  id: number;
  category: string;
  role: string;
  caption: string;
  style: string;
  scene: string;
}

// 精选 30 条表情包创意案例
const raw30Samples: MemeSample[] = [
  {
    id: 1,
    category: '职场玩梗',
    role: '经典熊猫头',
    caption: '又到了打工人最喜欢的星期五！',
    style: '经典黑白线条画',
    scene: '经典黑白熊猫头做出欢呼雀跃的夸张手势，头上满是欢庆的礼花。'
  },
  {
    id: 2,
    category: '科技巨头',
    role: '马斯克',
    caption: '发射火箭去火星开会，有事留言！',
    style: '3D 粘土卡通',
    scene: '马斯克骑在发光的火箭上，手握肥皂泡般的操控杆，眼神充满自信戏谑。'
  },
  {
    id: 3,
    category: '职场玩梗',
    role: '乔布斯',
    caption: 'One More Thing... 这个需求还得改！',
    style: '写实搞怪大头贴',
    scene: '乔布斯身穿经典黑领衫，指着前面光芒四射的产品图，露出迷之微笑。'
  },
  {
    id: 4,
    category: '社恐解压',
    role: '经典熊猫头',
    caption: '让我看看是谁又在群里偷懒摸鱼？',
    style: '经典黑白线条画',
    scene: '黑白熊猫头戴着特工墨镜，从墙角缓缓探出半个脑袋窥视。'
  },
  {
    id: 5,
    category: '科技巨头',
    role: '扎克伯格',
    caption: '进入元宇宙，没有人能打扰我！',
    style: '复古 8 位像素',
    scene: '像素风的扎克伯格头戴巨大 VR 头显，在虚拟方块世界中快乐翱翔。'
  },
  {
    id: 6,
    category: '生活搞笑',
    role: '比尔盖茨',
    caption: '钱不是万能的，但钱能解决 99% 的烦恼！',
    style: '3D 粘土卡通',
    scene: '比尔盖茨推了推粘土质感的大眼镜，手里拿着用金色光芒包裹的存折。'
  },
  {
    id: 7,
    category: '职场玩梗',
    role: '秦始皇',
    caption: '朕要大一统！先把这个 BUG 给我收复了！',
    style: '写实搞怪大头贴',
    scene: '秦始皇身穿黑金龙袍，手持巨型光剑，气势磅礴地指向一串报错代码。'
  },
  {
    id: 8,
    category: '生活搞笑',
    role: '爱因斯坦',
    caption: '快乐是相对的，但放假是绝对的！',
    style: '经典黑白线条画',
    scene: '爱因斯坦吐着舌头做出招牌表情，黑板上写着 E=mc² 和放假倒计时。'
  },
  {
    id: 9,
    category: '社恐解压',
    role: '柏拉图',
    caption: '退回理想国的洞穴里，静静看你们演戏。',
    style: '3D 粘土卡通',
    scene: '柏拉图披着白色粘土长袍，盘腿坐在温暖的篝火旁饮茶。'
  },
  {
    id: 10,
    category: '科技巨头',
    role: '贝索斯',
    caption: '亚马逊次日达，送货送到月球上！',
    style: '写实搞怪大头贴',
    scene: '光头贝索斯背着巨大的太空快递包裹，在大步跨越环形山。'
  },
  {
    id: 11,
    category: '职场玩梗',
    role: '经典熊猫头',
    caption: '需求改了十八遍，依然优雅保持微笑！',
    style: '经典黑白线条画',
    scene: '熊猫头挂着一滴巨大汗珠，嘴角勾起一丝僵硬但礼貌的笑容。'
  },
  {
    id: 12,
    category: '生活搞笑',
    role: '特斯拉',
    caption: '无线输电成功！我的快乐不需要插线！',
    style: '复古 8 位像素',
    scene: '像素风的尼古拉·特斯拉手握放电球，全身闪烁着蓝紫色的电火花。'
  },
  {
    id: 13,
    category: '社恐解压',
    role: '经典熊猫头',
    caption: '已开启已读不回模式，勿扰！',
    style: '经典黑白线条画',
    scene: '熊猫头把自己裹在厚厚的被子里，只露出一双无辜的小眼睛。'
  },
  {
    id: 14,
    category: '职场玩梗',
    role: '马斯克',
    caption: '今天不加班，今晚带大家去赛博朋克城拉力赛！',
    style: '3D 粘土卡通',
    scene: '马斯克驾驶着粘土质感的皮卡拖车，尾气喷射出五彩斑斓的光芒。'
  },
  {
    id: 15,
    category: '生活搞笑',
    role: '经典熊猫头',
    caption: '只要我睡得够快，贫穷就追不上我！',
    style: '经典黑白线条画',
    scene: '熊猫头安详地打着呼噜，头上浮现出金钱翅膀飞走的幻象。'
  },
  {
    id: 16,
    category: '科技巨头',
    role: '乔布斯',
    caption: '极简就是美，所以这个界面只保留一个按钮！',
    style: '写实搞怪大头贴',
    scene: '乔布斯托着下巴，面前悬浮着一个极其精致的极简玻璃按钮。'
  },
  {
    id: 17,
    category: '职场玩梗',
    role: '秦始皇',
    caption: '修筑长城防线，把产品经理的需求挡在门外！',
    style: '复古 8 位像素',
    scene: '8 位像素风的长城上，秦始皇挥舞旗帜防守城门。'
  },
  {
    id: 18,
    category: '社恐解压',
    role: '爱因斯坦',
    caption: '别跟我讲大道理，我的时空是弯曲的！',
    style: '3D 粘土卡通',
    scene: '爱因斯坦用粘土扭成一个莫比乌斯环，在环上滑滑梯。'
  },
  {
    id: 19,
    category: '生活搞笑',
    role: '经典熊猫头',
    caption: '吃饱了才有力气减重，再来一份宵夜！',
    style: '经典黑白线条画',
    scene: '圆滚滚的熊猫头左手拿汉堡右手拿奶茶，吃得满脸幸福。'
  },
  {
    id: 20,
    category: '科技巨头',
    role: '扎克伯格',
    caption: '代码更新完成，系统运行平稳！',
    style: '3D 粘土卡通',
    scene: '扎克伯格坐在发光的超级计算机前，比出一个大大的 OK 手势。'
  },
  {
    id: 21,
    category: '职场玩梗',
    role: '比尔盖茨',
    caption: '遇到蓝屏不要慌，重启能解决 90% 的问题！',
    style: '经典黑白线条画',
    scene: '盖茨微笑着按下一个巨大的金色 Reset 重启开关。'
  },
  {
    id: 22,
    category: '社恐解压',
    role: '柏拉图',
    caption: '真理在灵魂深处，不在吵闹的群聊里。',
    style: '写实搞怪大头贴',
    scene: '柏拉图戴着耳塞，坐在优雅的古希腊神殿长椅上静思。'
  },
  {
    id: 23,
    category: '生活搞笑',
    role: '贝索斯',
    caption: '今天也是被购物车清空治愈的一天！',
    style: '复古 8 位像素',
    scene: '像素风贝索斯推着巨大的像素购物车，里面塞满了各种各样的奖章。'
  },
  {
    id: 24,
    category: '职场玩梗',
    role: '特斯拉',
    caption: '不要限制我的想象力，我的脑波能发电！',
    style: '写实搞怪大头贴',
    scene: '特斯拉头上闪烁着灵感电光，手里画出复杂的机械蓝图。'
  },
  {
    id: 25,
    category: '社恐解压',
    role: '经典熊猫头',
    caption: '对方不想说话，并向你扔了一只猫咪！',
    style: '经典黑白线条画',
    scene: '熊猫头表情严肃，手里小心翼翼地捧着一只软萌小猫。'
  },
  {
    id: 26,
    category: '科技巨头',
    role: '马斯克',
    caption: '谁说星舰不能开去买烧烤？',
    style: '复古 8 位像素',
    scene: '8 位像素画风的星舰停在路边烧烤摊旁，马斯克在买考串。'
  },
  {
    id: 27,
    category: '生活搞笑',
    role: '经典熊猫头',
    caption: '道理我都懂，但我就是想偷懒！',
    style: '经典黑白线条画',
    scene: '熊猫头趴在桌子上，旁边放着一本没翻开的书。'
  },
  {
    id: 28,
    category: '职场玩梗',
    role: '乔布斯',
    caption: '改变世界，从今晚早点睡开始！',
    style: '3D 粘土卡通',
    scene: '乔布斯穿着粘土睡衣，抱着一个苹果造型的抱枕睡觉。'
  },
  {
    id: 29,
    category: '社恐解压',
    role: '秦始皇',
    caption: '免礼平身，朕今天只想当个快乐的肥宅！',
    style: '3D 粘土卡通',
    scene: '秦始皇穿着宽松粘土服，坐在舒适的大沙发上看电视。'
  },
  {
    id: 30,
    category: '生活搞笑',
    role: '爱因斯坦',
    caption: '知识就是力量，但奶茶能带来快乐！',
    style: '复古 8 位像素',
    scene: '像素风爱因斯坦一边手写公式，一边用吸管喝珍珠奶茶。'
  }
];

const samples = ref<MemeSample[]>(raw30Samples);

const filteredSamples = computed(() => {
  return samples.value.filter(s => {
    const matchCat = currentCategory.value === '全部' || s.category === currentCategory.value;
    const matchQuery = !searchQuery.value.trim() || 
      s.caption.includes(searchQuery.value) || 
      s.role.includes(searchQuery.value) || 
      s.style.includes(searchQuery.value) ||
      s.scene.includes(searchQuery.value);
    return matchCat && matchQuery;
  });
});

const totalPages = computed(() => Math.ceil(filteredSamples.value.length / pageSize) || 1);

const paginatedSamples = computed(() => {
  const start = (currentPage.value - 1) * pageSize;
  return filteredSamples.value.slice(start, start + pageSize);
});

watch([currentCategory, searchQuery], () => {
  currentPage.value = 1;
});
</script>
