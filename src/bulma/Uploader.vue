<template>
    <core-uploader v-bind="$attrs"
        v-on="$listeners"
        ref="uploader">
        <template v-slot:default="{
                compact, controlEvents, files, inputBindings, inputEvents, label, manual,
            }">
            <form class="is-marginless"
                @submit.prevent>
                <input class="is-hidden"
                    v-bind="inputBindings"
                    v-on="inputEvents">
                <slot name="control"
                    :control-events="controlEvents">
                    <a :class="['file', {'is-small': isSmall}, {'is-large': isLarge}]"
                        v-on="controlEvents">
                        <span :class="['file-cta', {'is-rounded': isRounded}]">
                            <span class="file-icon"
                                v-if="!manual || files">
                                <fa icon="upload"/>
                            </span>
                            <span class="file-label" v-if="!compact">
                                {{ label }}
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
import CoreUploader from '../renderless/CoreUploader.vue';

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
