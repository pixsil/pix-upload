<template>
    <div class="custom-file-input-div">
        <!--<div v-bind:class="{ 'has-error' : vueFormParent.vueErrors.get(field.column_name) }" @keydown="vueFormParent.vueErrors.clear(field.column_name)" class="form-group">-->
        <!--    <div class="row">-->
        <!--        <div class="col-md-12  font-weight-bold">-->
        <!--            <label class="col-form-label" :for="field.column_name">{{ field.label }}</label>-->
        <!--        </div>-->
        <!--    </div>-->
        <!--    <div class="row">-->
        <!--        <div class="col-md-12 ">-->
        <!--            <div v-if="$root.has_ftp">-->

        <div class="mb-3">
            <label for="formFile" class="form-label">Default file input example</label>
            <input class="form-control" type="file" id="formFile">
        </div>

        <div class="custom-file ">

            <div class="input-group mb-3">
                <label data-browse="Browse"
                    :for="this.field_name"
                    class="custom-file-label fake-form-control form-control"
                >
                    <span v-if="!has_file" class="d-block form-file-text" style="pointer-events: none;">
                        <template v-if="isFileInObject && vueFormParent.formData[this.field_name]['delete'] === true"><i class="mdi mdi-close-thick me-1"></i> Deleted uploaded file.</template>
                        <template v-else>No file selected.</template>
                    </span>
                    <span v-else-if="has_file && !is_uploaded_file" class="d-block form-file-text" style="pointer-events: none;">
                        <i class="mdi mdi-upload me-1"></i> File selected for upload.
                    </span>
                    <span v-else class="d-block form-file-text" style="pointer-events: none;">
                        <i class="mdi mdi-check-bold me-1"></i> File uploaded.
                    </span>
                </label>
                <input type="file"
                    class="custom-file-input"
                    @change="handeFileUpload"
                    :id="this.field_name">
                <span v-if="has_file" class="input-group-text mouse-pointer" @click="deleteFile">
                    <i class="mdi mdi-delete mdi-18px"></i>
                </span>
                <span v-if="is_uploaded_file" class="input-group-text mouse-pointer" @click="deleteFile">
                    <i class="mdi mdi-download mdi-18px"></i>
                </span>
            </div>


        </div>


        <!--<div class="input-group mb-3">-->

        <!--    <input class="form-control"-->
        <!--        type="file"-->
        <!--        :ref="this.field_name"-->
        <!--        :value="getFile(vueFormParent.formData, field_name)"-->
        <!--        @change="setFile(vueFormParent.formData, field_name, $event)"-->
        <!--    >-->
        <!--    <label for=""></label>-->
        <!--    <span v-if="has_file" class="input-group-text mouse-pointer" @click="deleteFile">-->
        <!--        <i class="mdi mdi-delete mdi-18px"></i>-->
        <!--    </span>-->
        <!--    <span v-if="is_uploaded_file" class="input-group-text mouse-pointer" @click="deleteFile">-->
        <!--        <i class="mdi mdi-download mdi-18px"></i>-->
        <!--    </span>-->
        <!--</div>-->

                        <!--<b-input-group>-->
                        <!--    <b-form-file-->
                        <!--        ref="upload_field"-->
                        <!--        @change="handeFileUpload"-->
                        <!--        :placeholder="placeholder"-->
                        <!--        drop-placeholder="Drop file here..."-->
                        <!--        class="mouse-pointer"-->
                        <!--    ></b-form-file>-->
                        <!--    <b-input-group-append v-if="has_file" @click="deleteFile" is-text class="mouse-pointer">-->
                        <!--        <i class="fa fa-sm fa-trash"></i>-->
                        <!--    </b-input-group-append>-->
                        <!--    <b-input-group-append v-if="is_uploaded_file" @click="openFile" is-text class="mouse-pointer">-->
                        <!--        <i class="fas fa-file-download"></i>-->
                        <!--    </b-input-group-append>-->
                        <!--</b-input-group>-->


    <!--                </div>-->
    <!--                <div v-else>-->
    <!--                    <div v-if="$root.is_admin" class="alert alert-light" role="alert">-->
    <!--                        There is no FTP connection configured.-->
    <!--                    </div>-->
    <!--                </div>-->
    <!--            </div>-->
    <!--        </div>-->
    <!--        <div class="row" v-if="vueFormParent.vueErrors.get(field.column_name)">-->
    <!--            <div class="col-md-12">-->
    <!--                <span class="text-danger small">-->
    <!--                    {{ vueFormParent.vueErrors.get(field.column_name) }}-->
    <!--                </span>-->
    <!--            </div>-->
    <!--        </div>-->
    <!--    </div>-->
    </div>
</template>

<script>
import PixUploadFieldFunctions from '../tools/pix-components/pix-file-field-functions.js'

export default {

    mixins: [
        PixUploadFieldFunctions
    ],

    props: {
        field_name: String,
        vueFormParent: Object,
    },

    data() {
        return {
        };
    },

    methods: {
        isObjectNoFile(variable) {
            // guard file becase file is an object
            if (variable instanceof File) {
                return false;
            }
            return (typeof variable === 'object' && !Array.isArray(variable) && variable !== null);
        },
        handeFileUpload(event) {
            // we are using an object
            if (this.isFileInObject) {
                this.vueFormParent.formData[this.field_name]['file'] = event.target.files[0];
            } else {
                this.vueFormParent.formData[this.field_name] = event.target.files[0];
            }
        },
        deleteFile() {
            // we are using an object
            if (this.isFileInObject) {
                this.vueFormParent.formData[this.field_name]['file'] = '';
                this.vueFormParent.formData[this.field_name]['delete'] = true;
            } else {
                this.vueFormParent.formData[this.field_name] = '';
            }
        },
        openFile() {
            window.open('/serve/'+ this.field.table_config_id +'/'+ this.field.id +'/'+ this.vueFormParent.formData.id, '_blank');
        },
    },

    mounted() {
    },

    created() {
    },

    computed: {
        isFileInObject() {
            return this.isObjectNoFile(this.vueFormParent.formData[this.field_name]);
        },
        // placeholder() {
        //     //
        //     if (typeof this.vueFormParent.formData[this.field_name] !== 'string') {
        //         return '';
        //     }
        //     //
        //     if (!this.vueFormParent.formData[this.field_name]) {
        //         return ''
        //     }
        //
        //     return this.vueFormParent.formData[this.field_name];
        // },
        has_file() {
            let result = false;
            // we are using an object
            if (this.isFileInObject) {
                if (this.vueFormParent.formData[this.field_name]['file']) {
                    result = true;
                }
            } else {
                if (this.vueFormParent.formData[this.field_name]) {
                    result = true;
                }
            }
            return result;
        },
        is_uploaded_file() {
            // guard false
            if (!this.vueFormParent.formData[this.field_name]) {
                return false;
            }

            let result = false;

            if (this.isFileInObject) {
                if (typeof this.vueFormParent.formData[this.field_name]['file'] === 'string') {
                    result = true;
                }
            } else {
                if (typeof this.vueFormParent.formData[this.field_name] === 'string') {
                    result = true;
                }
            }

            return result;
        },
        // field_var() {
        //     return this.vueFormParent.formData[this.field_name];
        // }
    },
    watch: {
        // 'field_var': function (newQuestion, oldQuestion) {
        //     // guard false
        //     if (!this.vueFormParent.formData[this.field_name]) {
        //         return;
        //     }
        //     //
        //     if (typeof this.vueFormParent.formData[this.field_name] !== 'string') {
        //         return;
        //     }
        //     // this.$refs[this.field_name].reset();
        // }
    },
}
</script>

<style>
.custom-file-input-div .custom-file-label {
    cursor: pointer;
}
.custom-file-input-div .custom-file {
    position: relative;
    display: inline-block;
    width: 100%;
    height: calc(1.6em + 0.75rem + 2px);
    margin-bottom: 0;
}
.custom-file-input-div .custom-file-input {
    position: relative;
    z-index: 2;
    width: 100%;
    height: calc(1.6em + 0.75rem + 2px);
    margin: 0;
    overflow: hidden;
    opacity: 0;
    display: none;
}
.custom-file-input-div .custom-file-label {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    z-index: 1;
    height: calc(1.6em + 0.75rem + 2px);
    padding-top: 0.375rem;
    padding-right: 0.75rem;
    padding-bottom: 0.375rem;
    padding-left: 1.75rem;
    overflow: hidden;
    font-weight: 400;
    line-height: 1.6;
    color: #495057;
    background-color: #fff;
    white-space: nowrap;
    overflow-x: hidden;
}
.custom-file-input-div .custom-file-label::after {
    position: absolute;
    top: 0;
    /*left: 0;*/
    bottom: 0;
    z-index: 3;
    display: block;
    height: calc(1.6em + 0.75rem);
    padding-top: 0.375rem;
    padding-bottom: 0.375rem;
    line-height: 1.6;
    color: #495057;
    content: "Browse…";
    background-color: #e9ecef;
    border-left: inherit;
    left: -1px;
    color: black;
}
.custom-file-input-div .fake-form-control {
    position: relative;
    flex: 1 1 auto;
    width: 1%;
    min-width: 0;
    margin-bottom: 0;
}
.form-file-text {
    padding-left: 88px;
}

</style>
