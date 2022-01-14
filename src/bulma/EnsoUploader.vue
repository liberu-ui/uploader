<template>
    <uploader v-bind="$attrs"
        :http="http"
        :i18n="i18n"
        ref="uploader"/>
</template>

<script>
import Uploader from './Uploader.vue';

export default {
    name: 'EnsoUploader',

    components: { Uploader },

    inject: ['http', 'i18n'],

    methods: {
        browseFiles() {
            this.$refs.uploader.browseFiles();
        },
    },
};
</script>
