<template>
    <uploader :i18n="__"
        v-bind="$attrs"
        v-on="$listeners"/>
</template>

<script>
import Uploader from './Uploader.vue';

export default {
    components: { Uploader },
};
</script>
