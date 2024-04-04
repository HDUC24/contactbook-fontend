<template>
    <div v-if="contact" class="page">
        <h4>Hieu chinh Lien He</h4>
        <contactForm :contact="contact" @submit:contact="submitContact" @delete:contact="deleteContact" />
        <p>{{ message }}</p>
    </div>
</template>

<script>
import contactForm from './components/contactForm.vue';
import contacrService from './services/contactService';

export default {
    components: {
        contactForm
    },
    props: {
        id: {
            type: String,
            required: true
        }
    },
    data() {
        return {
            contact: null,
            message: ''
        }
    },
    methods: {
        async getContact(id) {
            try {
                this.contact = await contactservice.get(id);
            } catch (err) {
                this.router.push({
                    name: 'notFound',
                    params:
                    {
                        pathMatch: this.$route.path.slit("/").slice(1)
                    },
                    query: this.$route.query,
                    hash: this.$route.hash
                })
            }
        }
    },

    async updateContact(data) {
        try {
            await contactService.update(this.contact._id, data);
            this.message = 'Cap nhat thanh cong';
        } catch (err) {
            this.message = 'Co loi xay ra';
        }
    },
    async deleteContact() {
        try {
            await contactService.delete(this.contact._id);
            this.message = 'Xoa thanh cong';
        } catch (err) {
            this.message = 'Co loi xay ra';
        }
    },
    created() {
        this.getContact(this.id);
        this.message = '';
    }

}
</script>