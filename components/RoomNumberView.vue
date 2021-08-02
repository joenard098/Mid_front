<template>
    <div>
        <button class="btn btn-info float-right" @click="onNew">
            New Item
        </button>
        <h5>Room Number View</h5>
        <hr>
        <form action="" @submit.prevent="onSave">
            <b-form-group label="Name:">
                <b-form-input v-model="reservation.name"></b-form-input>
            </b-form-group>
            <b-form-group label="RoomNumber:">
                <b-form-input v-model="reservation.roomnumber"></b-form-input>
            </b-form-group>
            <b-form-group label="Address:">
                <b-form-input v-model="reservation.address"></b-form-input>
            </b-form-group>
            <b-form-group label="Phone:">
                <b-form-input v-model="reservation.phone"></b-form-input>
            </b-form-group>
            <b-form-group label="No. of People:">
                <b-form-input v-model="reservation.quantity"></b-form-input>
            </b-form-group>
            <b-form-group>
               <button class="btn btn-primary" type="submit">Save Changes</button>
                <button class="btn btn-danger" type="button" @click="onDelete" v-if="reservation.id">Delete</button>
            </b-form-group>
        </form>
    </div>
</template>
<script>
export default {
    props: {
        reservation: {}
    },
    methods: {
        async onSave() {
            try {
                if(!this.reservation.id) {
                    await this.$axios.post('https://lentrix.tk/lmd/api/merchandises', this.reservation)
                }else {
                    await this.$axios.put('https://lentrix.tk/lmd/api/merchandises/'+this.reservation.id, this.reservation)
                }
                this.$emit('saved');
            }catch(err) {
                alert(err.response.data.message)
            }
        },
        onNew() {
            this.$emit('newItem')
        },
        async onDelete() {
            try {
                this.$axios.delete(`https://lentrix.tk/lmd/api/reservations/${this.reservation.id}`)
                this.$emit('deleted')
            }catch(err) {
                alert(err.response.data.message)
            }
        }
    }
}
</script>