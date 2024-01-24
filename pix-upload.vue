<!-- v2 added basic functionalitties no delete button etc -->
<template>
    <div class="custom-file b-form-file">
        <div class="input-group mb-3">
            <label data-browse="Browse" class="custom-file-label" :class="[ readOnly ? 'disabled' : '' ]" @click="handleClick">
                <span v-if="!has_file" class="d-block" style="pointer-events: none;">No file chosen</span>
                <span v-else-if="has_file && !is_uploaded_file" class="d-block" style="pointer-events: none;">File selected for upload</span>
                <span v-else class="d-block" style="pointer-events: none;">File uploaded</span>
            </label>
            <input type="file"
                   :disabled="readOnly"
                   accept="application/pdf"
                   class="custom-file-input"
                   ref="file"
                   style="z-index: -5;"
                   @change="selectedFile($event)">
        </div>
    </div>
</template>

<script>
export default {
    props: {
        modelValue: Object,
        readOnly: {
            type: Boolean,
            default: null,
        },
    },

    emits: [
        'update:modelValue',
    ],


    components: {},

    data() {
        return {}
    },

    methods: {
        handleClick() {
            if (this.readOnly) {
                return;
            }
            this.$refs.file.click()
        },
        selectedFile(event) {

            let file = event.target.files[0];

            var file_object = {
                'file': file,
                'delete': false,
            }

            this.$emit('update:modelValue', file_object)
        },
    },

    mounted() {

    },

    created() {
        // Event.('event', () => {});
    },


    computed: {
        has_file() {
            return this.modelValue?.file ? true : false;
        },
        is_uploaded_file() {
            // guard false
            if (!this.modelValue.file) {
                return false;
            }
            //
            if (typeof this.modelValue.file !== 'string') {
                return false;
            }

            return true;
        },
    }
}
</script>

<style scoped>
.disabled {
    background-color: #e9ecef;
}
</style>
