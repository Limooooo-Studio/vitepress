<script setup lang="ts">
/**
 * Limooo 页脚 —— 结构/样式对齐主站 Flask/src/templates/_footer.html。
 *
 * 这是 fork 定制件：不使用 VitePress 自带的 VPFooter。
 * 主站页面不滚动所以页脚 position:fixed；文档站要滚动，这里走正常文档流，
 * 其余（字体、字号、字距、分隔符、Baloo 2 品牌字）与主站一致。
 */
import { computed } from 'vue'
import { useData } from 'vitepress'

import { useLayout } from '../composables/layout'

const { theme, frontmatter } = useData()
const { hasSidebar } = useLayout()

interface FooterItem {
  text: string
  link?: string
}

const footer = computed<any>(() => (theme.value as any).footer ?? {})
const items = computed<FooterItem[]>(() => footer.value.items ?? [])
const show = computed(
  () => frontmatter.value.footer !== false && !!(footer.value.copyright || items.value.length)
)
</script>

<template>
  <footer
    v-if="show"
    class="VPLimoooFooter"
    :class="{ 'has-sidebar': hasSidebar }"
    id="global-footer"
  >
    <div class="footer-link">
      <div class="footer-text">
        <span
          v-if="footer.copyright"
          class="footer-item footer-copyright"
          v-html="footer.copyright"
        />
        <span
          v-for="(item, index) in items"
          :key="index"
          class="footer-item"
        >
          <a
            v-if="item.link"
            class="footer-source-link"
            :href="item.link"
            target="_blank"
            rel="noopener noreferrer"
          >{{ item.text }}</a>
          <template v-else>{{ item.text }}</template>
        </span>
      </div>
    </div>
  </footer>
</template>
