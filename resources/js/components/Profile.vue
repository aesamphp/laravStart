<template>
    <div class="container-fluid">
        <div class="row">
            

            <div class="col-md-12 mt-3">
                <!-- Widget: user widget style 1 -->
                <div class="card card-widget widget-user">
                <!-- Add the bg color to the header using any of the bg-* classes -->
                <div class="widget-user-header text-white" style="background-image: url('./img/user-cover.png')">
                    <h3 class="widget-user-username">{{ this.form.name }}</h3>
                    <h5 class="widget-user-desc">{{ this.form.type }}</h5>
                </div>
                <div class="widget-user-image">
                    <img class="img-circle" :src="getProfilePhoto()" :alt="form.name">
                </div>
                <div class="card-footer">
                    <div class="row">
                    <div class="col-sm-4 border-right">
                        <div class="description-block">
                        <h5 class="description-header">3,200</h5>
                        <span class="description-text">SALES</span>
                        </div>
                        <!-- /.description-block -->
                    </div>
                    <!-- /.col -->
                    <div class="col-sm-4 border-right">
                        <div class="description-block">
                        <h5 class="description-header">13,000</h5>
                        <span class="description-text">FOLLOWERS</span>
                        </div>
                        <!-- /.description-block -->
                    </div>
                    <!-- /.col -->
                    <div class="col-sm-4">
                        <div class="description-block">
                        <h5 class="description-header">35</h5>
                        <span class="description-text">PRODUCTS</span>
                        </div>
                        <!-- /.description-block -->
                    </div>
                    <!-- /.col -->
                    </div>
                    <!-- /.row -->
                </div>
                </div>
                <!-- /.widget-user -->
            </div>


            
            <div class="col-md-12">
            <div class="card">
              <div class="card-header p-2">
                <ul class="nav nav-pills">
                  <li class="nav-item"><a class="nav-link active" href="#settings" data-toggle="tab">Settings</a></li>
                  <li class="nav-item"><a class="nav-link" href="#activity" data-toggle="tab">Activity</a></li>
                  <li class="nav-item"><a class="nav-link" href="#timeline" data-toggle="tab">Timeline</a></li>
                </ul>
              </div><!-- /.card-header -->
              <div class="card-body">
                <div class="tab-content">
                  <div class="tab-pane" id="activity">
                    <h3>Display User Activity</h3>
                  </div>
                  <!-- /.tab-pane -->
                  <div class="tab-pane" id="timeline">
                    
                  </div>
                  <!-- /.tab-pane -->

                  <div class="active tab-pane" id="settings">
                    <form class="form-horizontal">
                      <div class="form-group">
                        <label for="inputName" class="col-sm-2 control-label">Name</label>

                        <div class="col-sm-10">
                          <input type="text" name="name" v-model="form.name" class="form-control" id="inputName" placeholder="Name" :class="{ 'is-invalid': form.errors.has('name') }">
                          <has-error :form="form" field="name"></has-error>
                        </div>
                      </div>
                      <div class="form-group">
                        <label for="inputEmail" class="col-sm-2 control-label">Email</label>

                        <div class="col-sm-10">
                          <input type="email" name="email" v-model="form.email" class="form-control" id="inputEmail" placeholder="Email" :class="{ 'is-invalid': form.errors.has('email') }">
                          <has-error :form="form" field="email"></has-error>
                        </div>
                      </div>
                      
                      <div class="form-group">
                        <label for="inputExperience" class="col-sm-2 control-label">Experience</label>

                        <div class="col-sm-10">
                          <textarea name="bio" v-model="form.bio" class="form-control" id="inputBio" placeholder="Bio" :class="{ 'is-invalid': form.errors.has('bio') }"></textarea>
                          <has-error :form="form" field="bio"></has-error>
                        </div>
                      </div>

                      <div class="form-group">
                        <label for="inputExperience" class="col-sm-2 control-label">Profile Photo</label>

                        <div class="col-sm-10">
                          <input type="file" @change="updateProfile" name="photo" class="form-control" id="inputExperience" placeholder="Experience"></input>
                        </div>
                      </div>

                      <div class="form-group">
                        <label for="inputName2" class="col-sm-4 control-label">Passport (leave empty if not changing)</label>

                        <div class="col-sm-10">
                          <input type="password" name="password" v-model="form.password" class="form-control" id="inputPassword" placeholder="Password" :class="{ 'is-invalid': form.errors.has('password') }">
                          <has-error :form="form" field="password"></has-error>
                        </div>
                      </div>


                      
                      <div class="form-group">
                        <div class="col-sm-offset-2 col-sm-10">
                          <div class="checkbox">
                            <label>
                              <input type="checkbox"> I agree to the <a href="#">terms and conditions</a>
                            </label>
                          </div>
                        </div>
                      </div>
                      <div class="form-group">
                        <div class="col-sm-offset-2 col-sm-12">
                          <button @click.prevent="updateInfo" type="submit" class="btn btn-danger">Update</button>
                        </div>
                      </div>
                    </form>
                  </div>
                  <!-- /.tab-pane -->
                </div>
                <!-- /.tab-content -->
              </div><!-- /.card-body -->
            </div>
            <!-- /.nav-tabs-custom -->
          </div>

            
                


            
        </div>
    </div>
</template>

<style>
.widget-user-header {
    background-position: center center;
    background-size: cover;
    height: 300px;
}
</style>
<script>
    export default {
        data() {
            return {
                form: new Form({
                    id : '',
                    name : '',
                    email : '',
                    password : '',
                    type : '',
                    bio : '',
                    photo : ''
                })
            }
        },
        methods : {
            updateInfo(){
                this.$Progress.start()
                if(this.form.password == ''){
                  this.form.password = undefined;
                }
                this.form.put('api/profile')
                .then(()=>{
                  Fire.$emit('AfterCreate');
                  
                  this.$Progress.finish()

                })
                .catch(()=>{
                  this.$Progress.fail()

                });
            },
            updateProfile(e){
                var file = e.target.files[0];
                var reader = new FileReader();

                if(file['size'] < 2111775){
                    reader.onloadend = (file) => {
                        //console.log('RESULT', reader.result)
                        this.form.photo = reader.result;
                    }
                    reader.readAsDataURL(file);
                }else{
                    Swal.fire(
                        'Oops...',
                        'You are uploading a large file',
                        'error'
                    )
                }
            },
            getProfilePhoto() {
              //let photo = (this.form.photo.indexOf('base64') != -1) ? this.form.photo : 'img/profile/' + this.form.photo;
              let photo = (this.form.photo.length > 200) ? this.form.photo : 'img/profile/' + this.form.photo;
              return photo;
            }
            

        },
        mounted() {
            console.log('Component mounted.')
        },
        created() {
            axios.get("api/profile").then(({ data }) => (this.form.fill( data )));
        }
    }
</script>
