<template>
  <div v-if="visible" class="modal-overlay" @click.self="$emit('close')">
    <div class="modal-content share-modal-content">
      <div class="modal-header">
        <h3>表情包创意卡片</h3>
        <button class="close-icon-btn" @click="$emit('close')">×</button>
      </div>

      <!-- 玻璃拟态分享卡片区域 (可截图或下载) -->
      <div id="share-card-node" class="share-card-wrapper">
        <div class="share-card-header">
          <div class="share-brand">
            <span class="brand-logo-dot"></span>
            <span class="brand-title">网腾无限AI</span>
          </div>
          <span class="share-tag">表情包生成器</span>
        </div>

        <div class="share-card-body image-card-body">
          <img v-if="imageUrl" :src="imageUrl" alt="AI 表情包图片" class="share-meme-img" />
          <div v-if="caption" class="share-meme-caption">“{{ caption }}”</div>
        </div>

        <div class="share-card-footer">
          <div class="share-qrcode-area">
            <div class="qr-placeholder">
              <svg xmlns="http://www.w3.org/2000/svg" width="36" height="36" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                <rect x="3" y="3" width="7" height="7" rx="1"></rect>
                <rect x="14" y="3" width="7" height="7" rx="1"></rect>
                <rect x="3" y="14" width="7" height="7" rx="1"></rect>
                <path d="M14 14h3v3h-3z"></path>
                <path d="M18 18h3v3h-3z"></path>
              </svg>
            </div>
            <div class="share-tip-text">
              <span class="primary">扫码体验 AI 表情包创意生成</span>
              <span class="sub">ai.wuxian.xyz · 微信公众号：{{ wechatId }}</span>
            </div>
          </div>
        </div>
      </div>

      <!-- 操作按钮 -->
      <div class="share-actions">
        <p class="share-guide-tip">微信 H5 环境下可长按图片保存，或直接截图保存此卡片到相册</p>
        <div class="share-btn-group">
          <button class="modal-btn secondary" @click="$emit('close')">关闭</button>
          <a v-if="imageUrl" :href="imageUrl" download="ai-meme.png" target="_blank" class="modal-btn primary-link">
            下载原图表情包
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  visible: boolean;
  imageUrl: string;
  caption: string;
  wechatId: string;
}>();

defineEmits<{
  (e: 'close'): void;
}>();
</script>
