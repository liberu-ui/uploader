<script>
import formatBytes from '../helpers/formatBytes';

export default {
    name: 'CoreUploader',

    inject: ['errorHandler'],

    props: {
        compact: {
            default: false,
            type: Boolean,
        },
        fileKey: {
            default: 'file',
            type: String,
        },
        fileSizeLimit: {
            default: 20 * 1024 * 1024,
            type: Number,
        },
        i18n: {
            default: (v) => v,
            type: Function,
        },
        label: {
            default: null,
            type: String,
        },
        manual: {
            default: false,
            type: Boolean,
        },
        multiple: {
            default: false,
            type: Boolean,
        },
        params: {
            default: null,
            type: Object,
        },
        url: {
            required: true,
            type: String,
        },
    },

    data: () => ({
        formData: new FormData(),
        succesfull: 0,
        files: false,
    }),

    computed: {
        displayLabel() {
            if (this.manual && this.files) {
                return this.i18n('Upload');
            }

            if (this.label) {
                return this.label;
            }

            return this.multiple
                ? this.i18n('File(s)')
                : this.i18n('File');
        },
        input() {
            return !!this.$el && this.$el.querySelector('input');
        },
    },

    methods: {
        browseFiles() {
            this.input.click();
            this.$emit('open-file-browser');
        },
        onChange(event) {
            if (!this.manual) {
                this.upload();
            } else {
                this.files = true;
                this.$emit('change', event);
            }
        },
        upload() {
            this.$emit('upload-start');
            this.setFormData();

            if (this.succesfull === 0) {
                return;
            }

            axios.post(this.url, this.formData).then((response) => {
                this.reset();
                this.$emit('upload-successful', response.data);
            }).catch((error) => {
                this.reset();
                this.$emit('upload-error');
                const { data, status } = error.response;

                if (status === 422) {
                    Object.keys(data.errors)
                        .forEach((key) => this.$toastr.error(data.errors[key][0]));
                    return;
                }

                this.errorHandler(error);
            });
        },
        setFormData() {
            const { files } = this.input;
            this.addFiles(files);

            if (this.succesfull > 0) {
                this.addParams();
            }
        },
        addFiles(files) {
            if (!this.multiple) {
                this.addFile(this.fileKey, files[0]);
                return;
            }

            for (let i = 0; i < files.length; i++) {
                if (this.sizeCheckPasses(files[i])) {
                    this.addFile(`${this.fileKey}_${i}`, files[i]);
                    this.succesfull++;
                }
            }
        },
        addFile(key, file) {
            if (this.sizeCheckPasses(file)) {
                this.formData.append(key, file);
                this.succesfull++;
            }
        },
        addParams() {
            if (this.params) {
                Object.entries(this.params).forEach(([key, param]) => {
                    const value = typeof param === 'object'
                        ? JSON.stringify(param)
                        : param;

                    this.formData.append(key, value);
                });
            }
        },
        sizeCheckPasses(file) {
            if (file.size > this.fileSizeLimit) {
                this.$toastr.warning(
                    `File size Limit of ${formatBytes(this.fileSizeLimit, 2)} exceeded by ${file.name}`,
                );
                return false;
            }

            return true;
        },
        reset() {
            this.$el.reset();
            this.formData = new FormData();
            this.succesfull = 0;
            this.files = false;
        },

    },

    render() {
        return this.$scopedSlots.default({
            compact: this.compact,
            controlEvents: {
                click: this.browseFiles,
            },
            files: this.files,
            inputBindings: {
                multiple: this.multiple,
                type: 'file',
            },
            inputEvents: {
                change: this.onChange,
                input: (event) => this.$emit('input', event.target.value),
            },
            label: this.displayLabel,
            multiple: this.multiple,
            manual: this.manual,
            upload: this.upload,
        });
    },
};
</script>
