<script>
import {defineComponent} from "vue";
import Child from "./components/child.vue";

export default defineComponent({
    components: {Child}
})

</script>

<template>
    <Suspense>
        <template #default>
            <Child></Child>
        </template>
    </Suspense>
</template>

<style scoped>

</style>
