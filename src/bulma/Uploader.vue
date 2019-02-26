<template>
    <renderless-uploader v-bind="$attrs"
        v-on="$listeners">
        <template v-slot:default="{ label, inputBindings, inputEvents, controlEvents }">
            <form @submit.prevent>
                <input class="is-hidden"
                    v-bind="inputBindings"
                    v-on="inputEvents">
                <slot name="control"
                    :control-events="controlEvents">
                    <a :class="[{'is-small': isSmall}, {'is-large': isLarge}]"
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
    </renderless-uploader>
</template>

<script>
import { library } from '@fortawesome/fontawesome-svg-core';
import { faUpload } from '@fortawesome/free-solid-svg-icons';
import RenderlessUploader from '../renderless/Uploader.vue';

library.add(faUpload);

export default {
    components: { RenderlessUploader },
    props: {
        isLarge: {
            type: Boolean,
            default: false,
        },
        isSmall: {
            type: Boolean,
            default: false,
        },
    },
};
</script>
