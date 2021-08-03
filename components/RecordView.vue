<template>
    <div>
        <button class="btn btn-success float-right" @click="onNew">
            New Record
        </button>
        <h5>Record View</h5>
        <hr>

        <form action="" @submit.prevent="onSave">
                <b-form-group label="Name" label-for="name">
                <b-form-input id="name" v-model="record.name" trim></b-form-input>
                </b-form-group>

                <b-form-group label="Age" label-for="age">
                <b-form-input id="age" v-model="record.age" trim></b-form-input>
                </b-form-group>
                

                <b-form-group label="Skills" label-for="skills">
                <b-form-input id="skills" v-model="record.skills" trim></b-form-input>
                </b-form-group>


                <b-form-group label="Address" label-for="address">
                <b-form-input id="address" v-model="record.address" trim></b-form-input>
                </b-form-group>

                <b-form-group label="Designation" label-for="designation">
                <b-form-select v-model="record.designation" :options="options"></b-form-select>
                </b-form-group>
                <b-form-group>
                <button class="btn btn-primary" type="submit">Save Changes</button>
                <button class="btn btn-danger float-right" type="button" @click="onDelete" v-if="record.id">Delete</button>
            </b-form-group>
            </form>
    </div>
</template>

<script>
export default {
    props:{
        record: {},
    },
    
    data() {
        return {
            record: {},
            options: [
                {value: 'programmer', text: 'Programmer'},
                {value: 'designer', text: 'Designer'},
            ]
        }
    },
     methods: {
        async onSave() {
            try {

                if(!this.record.id) {
                    this.$axios.post('http://localhost:8000/api/records', this.record)

                }else{
                    this.$axios.put('http://localhost:8000/api/records/' + this.record.id, this.record)

                }

                this.$emit('saved');

            } catch (error) {
                alert(error.response.data.message)
            }
        },

        onNew() {
            this.$emit('newRecord')
        },

        async onDelete() {
            try {
                this.$axios.delete('http://localhost:8000/api/records/' + this.record.id)

                this.$emit('deleted')
                
            } catch (error) {
                alert(error.response.data.message)
            }
            
        }
    }
}

</script>