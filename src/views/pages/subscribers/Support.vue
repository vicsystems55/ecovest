<template>
   <div class="nk-content-body">
        <div class="nk-content-wrap">
            <div class="nk-block-head nk-block-head-lg">
                <div class="nk-block-between-md g-4">
                    <div class="nk-block-head-content">
                        <h2 class="nk-block-title fw-normal">Support</h2>
                          <router-link v-if="admin" to="/admin/add-project" class="btn btn-primary">Add Project</router-link>
                       
                    </div>
                </div>
            </div><!-- .nk-block-head -->
         
        </div>

        <div class="container">

           <div class="row">
                <div class="col-3">
                    <div class="nav flex-column nav-pills" id="v-pills-tab" role="tablist" aria-orientation="vertical">
                    <a class="nav-link active" id="v-pills-home-tab" data-toggle="pill" href="#v-pills-home" role="tab" aria-controls="v-pills-home" aria-selected="true">New ticket*</a>
                    <a class="nav-link" id="v-pills-profile-tab" data-toggle="pill" href="#v-pills-profile" role="tab" aria-controls="v-pills-profile" aria-selected="false">Open Tickets</a>
                    <a class="nav-link" id="v-pills-messages-tab" data-toggle="pill" href="#v-pills-messages" role="tab" aria-controls="v-pills-messages" aria-selected="false">Answered Tickets</a>
                    <a class="nav-link" id="v-pills-settings-tab" data-toggle="pill" href="#v-pills-settings" role="tab" aria-controls="v-pills-settings" aria-selected="false">My Replies</a>
                    <a class="nav-link" id="v-pills-settings-tab" data-toggle="pill" href="#v-pills-settings" role="tab" aria-controls="v-pills-settings" aria-selected="false">Closed Tickes</a>
                    </div>
                </div>
                <div class="col-9">
                    <div class="tab-content" id="v-pills-tabContent">
                    <div class="tab-pane fade show active" id="v-pills-home" role="tabpanel" aria-labelledby="v-pills-home-tab">
                        
                        <h4>Create New</h4>

                        <div class="form-group">
                            <label for="">Title of Ticket</label>
                            <input type="text" v-model="title" class="form-control">
                        </div>

                        <div class="form-group">
                            <label for="">Department</label>
                            <select v-model="department" id="" class="form-control">
                                <option value="Management [Apply for payment extension]">Management [Apply for payment extension]</option>
                                <option value="Sales / Billing">Sales / Billing</option>
                                <option value="Relationship Manager">Relationship Manager</option>
                                <option value="Customer Care">Customer Care</option>
                            </select>
                        </div>

                        <div class="form-group">
                            <label for="">Message</label>
                            <textarea v-model="body" id="" cols="30" rows="10" class="form-control"></textarea>
                        </div>

                        <div class="form-group">
                            <label for="">Attachments (pdf, docx, jpeg, png, gif)</label>
                            <input @change="handleFileUpload()" type="file" ref="file" class="form-control-file" >
                        </div>

                        <div class="form-group">
                            <button @click="createTicket()" class="btn btn-primary shadow float-right">Submit</button>
                        </div>


                        
                    </div>
                    <div class="tab-pane fade" id="v-pills-profile" role="tabpanel" aria-labelledby="v-pills-profile-tab">
                        
                        <!-- <div class="accordion" id="accordionExample">


                                <div class="card" v-for="open_ticket in open_tickets" :key="open_ticket.index">


                                    <div class="card-header" v-bind:id="open_ticket.id+'nm'">
                                        <h2 class="mb-0">
                                            <button class="btn btn-link btn-block text-left" type="button" data-toggle="collapse" v-bind:data-target="'#'+open_ticket.id+'nn'" aria-expanded="false" v-bind:aria-controls="open_ticket.id+'nn'">
                                            {{open_ticket.title}}
                                            </button>
                                        </h2>
                                    </div>

                                    <div v-bind:id="open_ticket.id+'nn'" v-bind:class="' collapse '+(open_ticket.id == 2?'show':'')" v-bind:aria-labelledby="open_ticket.id+'nm'" data-parent="#accordionExample">
                                        <div class="card-body">
                                            {{open_ticket.body}}

                                            ddddd
                                        </div>
                                    </div>


                                </div>
                              
                            </div> -->
                                <div class="accordion" id="accordionExample">
                                <div class="card" v-for="open_ticket in open_tickets" :key="open_ticket.index">
                                    <div class="card-header" id="headingOne">
                                    <h2 class="mb-0">
                                        <button class="btn btn-link btn-block text-left" type="button" data-toggle="collapse" v-bind:data-target="'#'+'collapse'+open_ticket.id+'real'" aria-expanded="false" v-bind:aria-controls="'collapse'+open_ticket.id+'real'">
                                        {{open_ticket.title}}
                                        </button>
                                    </h2>
                                    </div>

                                    <div v-bind:id="'collapse'+open_ticket.id+'real'" class="collapse " aria-labelledby="headingOne" data-parent="#accordionExample">
                                    <div class="card-body">
                                        {{open_ticket.body}}
                                    </div>
                                    </div>
                                </div>

                                </div>
                    </div>
                    <div class="tab-pane fade" id="v-pills-messages" role="tabpanel" aria-labelledby="v-pills-messages-tab">
                        
                        ...
                        
                    </div>
                    <div class="tab-pane fade" id="v-pills-settings" role="tabpanel" aria-labelledby="v-pills-settings-tab">
                        
                        ...
                        
                    </div>
                    </div>
                </div>
            </div> 

      


        </div>
        <!-- footer @s -->
      
        <!-- footer @e -->
    </div>
</template>
<script>


export default {

    data() {
        return {
            title: '',
            body: '',
            department: '',
            file: '',
            open_tickets: []

        }
    },

    methods: {
        createTicket(){

            let loader = this.$loading.show({
                // Optional parameters
                container: this.fullPage ? null : this.$refs.formContainer,
                canCancel: false,
                onCancel: this.onCancel,
                color: '#6CC3EC',
            });


            let formData = new FormData();

            formData.append('attachements', this.file);

            formData.append('title', this.title);

            formData.append('body', this.body);

            formData.append('department', this.department);


             this.axios({
                url: process.env.VUE_APP_URL+'/api/tickets',
                method: 'post',
                data: formData,
                headers:{
                    'Content-Type': 'multipart/form-data',
                    'Access-Control-Allow-Origin': '*',
                  
                    'Authorization': 'Bearer ' +localStorage.getItem('user_token')

                }
            })
            .then((response)=>{

             loader.hide()

                console.log(response)
                this.getOpenTickets()

                 return this.$router.push('/user/ticket-success')
            })
            .catch((response)=>{

                        loader.hide()

                console.log(response)
            })

        },
        handleFileUpload(){
             this.file = this.$refs.file.files[0];

             console.log(this.file)
        },
        getOpenTickets(){

    
            this.axios({
                url: process.env.VUE_APP_URL+'/api/tickets',
                method: 'get',
                headers:{
                        'Access-Control-Allow-Origin': '*',
                        'Content-type': 'application/json',
                        'Accept': 'application/json',
                        'Authorization': 'Bearer ' +localStorage.getItem('user_token')
                },
                params:{
                    type: 'open'
                }
            })
            .then((response)=>{
                this.open_tickets = response.data
                console.log(response.data)
            })
             .catch((response)=>{

                console.log(response)
            })



        }
    },
    

    mounted() {
        // alert('im here')
        this.getOpenTickets()
    },


}
</script>