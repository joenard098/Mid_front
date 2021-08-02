<template>
    <div>
        <NavBar />
        <div class="container">
        <div class="row">
            <div class="col-6">
                <h5>List of Room Numbers</h5>
                <ul class="list-group">
                    <li class="list-group-item list-group-item-action" v-for="reservation in reservations" :key="reservation.id" @click="onSelected(merchandise)">
                        {{reservation.roomnumber}} <span class="float-right">{{reservation.phone}}</span>
                    </li>
                    
                </ul>
            </div>
            <div class="col-4">
                <RoomNumberView :reservation="selectedReservation" @saved="onChanges" @newItem="onNew" @deleted="onChanges"/>
            </div>
        </div>
    </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            reservation: [],
            selectedReservation: {}
        }
    },
    methods: {
        async getMyReservations() {
            await this.$axios.get('https://lentrix.tk/lmd/api/reservations')
            .then((res)=>{
                if(res.status==200) {
                    this.reservations = res.data
                }
            })
        },
        onChanges() {
            this.getMyReservations()
            this.selectedReservation = {}
        },
        onSelected(reservation) {
            this.selectedReservation = reservation
        },
        onNew() {
            this.selectedReservation = {}
        }
    },
    created() {
        this.getMyReservations()
    }
}
</script>
<style scoped>
.container{
    margin-top: 50px;
}
</style>