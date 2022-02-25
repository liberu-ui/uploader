<template>
    <form class="is-marginless"
        @submit.prevent>
        <core-uploader v-bind="$attrs"
            ref="uploader">
            <template #default="{
                controlEvents, files, inputBindings, inputEvents, label, manual,
            }">
                <input class="is-hidden"
                    v-bind="inputBindings"
                    v-on="inputEvents">
                <slot name="control"
                    :control-events="controlEvents">
                    <a :class="['file', {'is-small': isSmall}, {'is-large': isLarge}]"
                        v-on="controlEvents">
                        <span :class="['file-cta', {'is-rounded': isRounded}]"
                            v-if="label">
                            <span class="file-icon mx-a"
                                v-if="!manual || files">
                                <fa icon="upload"/>
                            </span>
                            <span class="file-label">
                                {{ label }}
                            </span>
                        </span>
                        <span class="button"
                            v-else>
                            <span class="icon"
                                v-if="!manual || files">
                                <fa icon="upload"/>
                            </span>
                        </span>
                    </a>
                </slot>
            </template>
        </core-uploader>
    </form>
</template>

<script>
import { FontAwesomeIcon as Fa } from '@fortawesome/vue-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faUpload } from '@fortawesome/free-solid-svg-icons';
import CoreUploader from '../renderless/CoreUploader.vue';

library.add(faUpload);

export default {
    name: 'Uploader',

    components: { CoreUploader, Fa },

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
