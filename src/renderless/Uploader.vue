<script>
export default {
    props: {
        fileKey: {
            type: String,
            default: 'file',
        },
        fileSizeLimit: {
            type: Number,
            default: 20000000,
        },
        i18n: {
            type: Function,
            default: v => v,
        },
        multiple: {
            type: Boolean,
            default: false,
        },
        params: {
            type: Object,
            default: null,
        },
        url: {
            type: String,
            required: true,
        },
    },

    data: () => ({
        input: null,
        formData: new FormData(),
        succesfull: 0,
    }),

    computed: {
        label() {
            return this.multiple
                ? this.i18n('File(s)')
                : this.i18n('File');
        },
    },

    methods: {
        browseFiles() {
            this.$refs.querySelector('input').click();
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
                        .forEach(key => this.$toastr.error(data.errors[key][0]));
                    return;
                }

                this.handleError(error);
            });
        },
        setFormData() {
            const { files } = this.$refs.input;
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
                this.$toastr.warning(`File size Limit of ${this.fileSizeLimit} Kb exceeded by ${file.name}`);
                return false;
            }

            return true;
        },
        reset() {
            this.$el.reset();
            this.formData = new FormData();
            this.succesfull = 0;
        },
    },

    render() {
        return this.$scopedSlots.default({
            label: this.label,
            multiple: this.multiple,
            upload: this.upload,
            inputBindings: {
                multiple: this.multiple,
                type: 'file'
            },
            inputEvents: {
                change: this.upload
            },
            controlEvents: {
                click: this.browseFiles
            }
        })
    }
};
</script>
