<template>
    <uploader :http="http"
        :i18n="i18n"
        ref="uploader">
        <template #control="props">
            <slot name="control"
                v-bind="props"/>
        </template>
    </uploader>
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
