<template>

    <div>

        <NavBar />

        <div class="container">

            <h1>List of Records</h1>

            <div class="row">

                <div class="col-6">

                    <h5>List of Records</h5>
                    <ul class="list-group">
                        <li class="list-group-item list-group-item-action" v-for="record in records" :key="record.id" @click="onSelected(record)">
                            {{record.name}} <span class="float-right">{{record.designation}}</span>
                        </li>
                    </ul>

                </div>

                <div class="col-4">
                    <RecordView :record="selectedRecord" @saved="onSave" @newRecord="onNew" @deleted="onDelete" />
                </div>

            </div>

        </div>

    </div>

</template>

<script>

export default {
    data() {
        return {
            records: [],
            selectedRecord: {}
        }
    },
    methods: {
        async getAll() {
            await this.$axios.get('http://localhost:8000/api/records')
            .then((res)=>{
                if(res.status==200) {
                    this.records = res.data
                    console.log(this.records)
                }
            })
        },

        onSaveo() {
            this.getAll()
            this.selectedRecord = {}
        },

        onSelected(record) {
            this.selectedRecord = record
        },

        onNew() {
            this.selectedRecord = {}
        },
        onDelete() {
            this.getAll()
        }
    },

    created() {
        this.getAll()
        this.selectedRecord = {}
    }
}

</script>
