<template>
    <core-uploader v-bind="$attrs"
        v-on="$listeners"
        ref="uploader">
        <template v-slot:default="{ label, inputBindings, inputEvents, controlEvents }">
            <form class="is-marginless"
                @submit.prevent>
                <input class="is-hidden"
                    v-bind="inputBindings"
                    v-on="inputEvents">
                <slot name="control"
                    :control-events="controlEvents">
                    <a :class="['file', {'is-small': isSmall}, {'is-large': isLarge}, {'is-rounded': isRounded}]"
                        v-on="controlEvents">
                        <span class="file-cta">
                            <span class="file-icon">
                                <fa icon="upload"/>
                            </span>
                            <span>
                                {{ label }}…
                            </span>
                        </span>
                    </a>
                </slot>
            </form>
        </template>
    </core-uploader>
</template>

<script>
import { library } from '@fortawesome/fontawesome-svg-core';
import { faUpload } from '@fortawesome/free-solid-svg-icons';
import CoreUploader from '../renderless/Uploader.vue';

library.add(faUpload);

export default {
    name: 'Uploader',

    components: { CoreUploader },

    props: {
        isLarge: {
            type: Boolean,
            default: false,
        },
        isRounded: {
            type: Boolean,
            default: false,
        },
        isSmall: {
            type: Boolean,
            default: false,
        },
    },

    methods: {
        browseFiles() {
            this.$refs.uploader.browseFiles();
        },
    },
};
</script>

<style lang="scss">
    .file-cta.is-rounded {
        border-radius: 290486px;
    }
</style>
