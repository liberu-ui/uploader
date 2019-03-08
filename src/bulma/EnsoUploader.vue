<template>
    <uploader :i18n="i18n"
        v-bind="$attrs"
        v-on="$listeners"/>
</template>

<script>
import Uploader from './Uploader.vue';

export default {
    name: 'Uploader',

    components: { Uploader },

    inject: ['i18n'],
};
</script>
