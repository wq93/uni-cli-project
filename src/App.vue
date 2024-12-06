<script setup>
import { onLaunch } from '@dcloudio/uni-app';

onLaunch((option) => {
  console.log(option, 'onLaunch');
});
</script>

<style>
/*每个页面公共css */
.red {
  background-color: red;
}

.yellow {
  background-color: yellow;
}
</style>
