### Box shadow on scroll

```js
<template>
  <div
    class="compare-section-head"
    :class="{ scrolled: isScrolled }"
    @scroll="handleScroll"
    ref="compareSection"
  >
    <!-- Content of your compare-section-head -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      isScrolled: false,
    };
  },
  methods: {
    handleScroll() {
      if (this.$refs.compareSection.scrollTop > 0) {
        this.isScrolled = true;
      } else {
        this.isScrolled = false;
      }
    },
  },
};
</script>

<style>
.compare-section-head {
  @apply flex items-center;
  height: 300px;
  position: sticky;
  left: 0;
  top: 0;
  right: 0;
  background-color: white;
  z-index: 1; /* Ensures the shadow is below other elements */
  transition: box-shadow 0.3s ease; /* Smooth transition for the box-shadow */
}

.compare-section-head.scrolled {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Box shadow when scrolled */
}
</style>
```
