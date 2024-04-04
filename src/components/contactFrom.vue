<template>
    <Form @submit="submitContact" :validation-schema="contactFormSchema">
        <div class="form-group">
            <label for="name">Ten</label>
            <Field type="text" name="name" class="form-control" v-model="contactLocal.name" />
            <ErrorMessage name="name" class="error-feedback" />

        </div>
        <div class="form-group">
            <label for="email">Email</label>
            <Field type="email" name="email" class="form-control" v-model="contactLocal.email" />
            <ErrorMessage name="email" class="error-feedback" />
        </div>
        <div class="form-group">
            <label for="phone">So dien thoai</label>
            <Field type="tel" name="phone" class="form-control" v-model="contactLocal.phone" />
            <ErrorMessage name="phone" class="error-feedback" />
        </div>
        <div class="form-group">
            <label for="address">Dia chi</label>
            <Field type="text" name="address" class="form-control" v-model="contactLocal.address" />
            <ErrorMessage name="address" class="error-feedback" />
        </div>
        <div class="form-group form-check">
            <input name="favorite" type="checkbox" class="form-check-input" v-model="contactLocal.favorite" />
            <label for="favorite" class="form-check-label">Yeu thich</label>
        </div>
        <div class="form-group">
            <button type="submit" class="btn btn-primary">Luu</button>
            <button v-if="contactLocal && contactLocal._id" type="button" class="btn btn-danger"
                @click="deleteContact">Xoa</button>
        </div>
    </Form>



</template>


<script>
import * as yup from 'yup';
import { Form, Field, ErrorMessage } from 'vee-validate';

export default {
    components: {
        Form,
        Field,
        ErrorMessage
    },
    emits: ['submit:contact', 'delete:contact'],
    props: {
        contact: {
            type: Object,
            required: true,
        }
    },
    data() {
        const contactFromSchema = yup.object({
            name: yup.string().required('Ten khong duoc de trong')
                .min(3, 'Ten phai co it nhat 3 ky tu')
                .max(50, 'Ten khong duoc qua 50 ky tu'),
            email: yup.string().email('Email khong hop le').required('Email khong duoc de trong')
                .max(50, 'Email khong duoc qua 50 ky tu'),
            phone: yup.string()
                .matches(/(84|0[3|5|7|8|9])+([0-9]{8})\b/, 'So dien thoai khong hop le'),
            address: yup.string().required('Dia chi khong duoc de trong')
                .max(100, 'Dia chi khong duoc qua 100 ky tu'),

        });
        return {
            contactLocal: { ...this.contact },
            contactFormSchema: contactFromSchema
        };
    },
    methods: {
        submitContact() {

            this.$emit('submit:contact', this.contactLocal);
        },
        deleteContact() {
            this.$emit('delete:contact', this.contactLocal);
        },
    },


}
</script>
<style scoped>
@import "@/assets/css/form.css";
</style>