<template>
   <!-- begin mini blog -->
   <div  id="page-content">
      <div class="padding">
         <div class="row container d-flex justify-content-center">
            <div class="col-md-12">
               <div class="card px-3 m-3">
                  <div class="card-body">
                      <!-- début formulaire d'ajout d'une nouvelle tâche -->
                     <form @submit.prevent="saveData">
                        <div class="add-items d-flex">
                           <input type="text" class="form-control" placeholder="Taper un statut !" v-model="form.content" :class="{'is-invalid' : form.errors.has('content')}"   @keydown="form.errors.clear('content')">
                           <button class="add btn btn-primary font-weight-bold ">Ajouter</button>

                        </div>
                        <!-- message d'erreur en cas de champ vide -->
                        <span class="text-danger pt-3 pb-3" style="font-size:16px;" v-if="form.errors.has('content')" v-text="form.errors.get('content')"></span>
                     </form>
                     <!-- fin formulaire -->

                   

                  </div>
               </div>
            </div>
         </div>
       <!-- card post -->
    <div class="row">
        <div class="col-md-12">
            <div class="post-content" v-for="post in posts" :key="post.id">
              <div class="post-container">
                <img src="https://bootdey.com/img/Content/avatar/avatar6.png" alt="user" class="profile-photo-md pull-left">
                <div class="post-detail">
                  <div class="user-info">
                    <h5><a href="#" class="profile-link">{{post.user.name}}</a></h5>
                    <p class="text-muted">Publié le : {{post.published_at}}</p>
                  </div>
                  <div class="reaction">
                    

                    </a>
                     <span style="cursor:pointer">
                                  <!-- icône suppression d'une tâche par l'utilisateur autorisé -->
                                 <svg v-if="post.user_id == iduser" v-on:click="deletePost(post)" xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-trash ml-1" width="36" height="36" viewBox="0 0 24 24" stroke-width="1.5" stroke="#FF5722" fill="none" stroke-linecap="round" stroke-linejoin="round">
                                    <path stroke="none" d="M0 0h24v24H0z"/>
                                    <line x1="4" y1="7" x2="20" y2="7" />
                                    <line x1="10" y1="11" x2="10" y2="17" />
                                    <line x1="14" y1="11" x2="14" y2="17" />
                                    <path d="M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2 -2l1 -12" />
                                    <path d="M9 7v-3a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v3" />
                                 </svg>
                              </span>
                  </div>
                  <div class="line-divider"></div>
                  <div class="post-text">
                    <p>{{post.content}} <i class="em em-anguished"></i> <i class="em em-anguished"></i> <i class="em em-anguished"></i></p>
                  </div>
                  <h5>Commentaires : </h5>
                  <div class="line-divider"></div>
                  <div class="post-comment post-cont" v-for="comment in post.comments" :key="comment.id" >
                    <div>
                    <p><a href="#" class="profile-link">{{comment.user.name}}</a> : {{comment.comment_content}} </p>
                   </div>
                   <div>
                    <p class="">{{comment.comment_date}} 
                      <a href="#" v-if="comment.user_id == iduser" @click.prevent="deleteComment(comment)"> - Supprimer</a>
                    </p>
                   </div>
                  </div>
                    
                  <form @submit.prevent="saveDataComment(post)">
                  <div class="post-comment">
                    <img src="https://bootdey.com/img/Content/avatar/avatar1.png" alt="" class="profile-photo-sm">
                    <input type="text" class="form-control" placeholder="Poster votre commentaire !!" v-model="form_comment.comment_content" :class="{'is-invalid' : form_comment.errors.has('comment_content')}"   @keydown="form_comment.errors.clear('comment_content')">
                  </div>
                  </form>
                </div>
              </div>
            </div>
        </div>
    </div>

       <!-- end card post -->

      </div>
      <!-- end mini blog -->
   </div>
</template>

<script>
    export default {
        data(){
            return{
                editmode: false,
                posts:'',
                comments:'',
                form_comment:new Form({
                  comment_content:'',
                  user_id:'',
                  post_id:'',
                  comment_date:'',
                }),
                form: new Form({
                    content: '',
                    user_id: '',
                    published_at:'',
                }),
                iduser:document.querySelector("meta[name='user-id']").getAttribute('content')
            }
        },
        methods:{
            currentDateTime() {
              const current = new Date();
              const date = current.getFullYear()+'-'+(current.getMonth()+1)+'-'+current.getDate();
              const time = current.getHours() + ":" + current.getMinutes() + ":" + current.getSeconds();
              const dateTime = date +' '+ time;
              return dateTime;
            },
            //************* methods for posts*/
          
            deletePost(e){
                let data = new FormData();
                data.append('_method', 'DELETE')
                axios.post('/api/post/'+e.id, data).then((res) =>{
                    this.posts = res.data
                }).catch((error) => {
                    this.form.errors.record(error.response.data.errors)
                })
            },
           

            
            getPosts(){
                    axios.get('/api/post').then((res) =>{
                        this.posts = res.data
                    }).catch((error) =>{
                        console.log(error)
                    })
            },

            saveData(){
                let data = new FormData();

                data.append('content', this.form.content)
                data.append('published_at',this.currentDateTime())
                data.append('user_id', this.iduser)
                axios.post('/api/post', data).then((res) =>{
                    this.form.reset()
                     this.getPosts()
                }).catch((error) => {
                    this.form.errors.record(error.response.data.errors)
                })
            },
        
          //*********** end methods posts*/

            //************* methods for comments*/
          
            deleteComment(e){
                let data = new FormData();
                data.append('_method', 'DELETE')
                axios.post('/api/comment/'+e.id, data).then((res) =>{
                    this.posts = res.data
                }).catch((error) => {
                    this.form.errors.record(error.response.data.errors)
                    
                })
            },

            
            getComments(){
                    axios.get('/api/comment').then((res) =>{
                        this.comments = res.data
                    }).catch((error) =>{
                        console.log(error)
                    })
            },

            saveDataComment(e){
                let data = new FormData();

                data.append('comment_content', this.form_comment.comment_content)
                data.append('comment_date',this.currentDateTime())
                data.append('user_id', this.iduser)
                data.append('post_id', e.id)
                axios.post('/api/comment', data).then((res) =>{
                    this.form_comment.reset()
                    this.getPosts()
                }).catch((error) => {
                    this.form.errors.record(error.response.data.errors)
                })
            },
        },
          //*********** end methods posts*/
        mounted() {
          this.getPosts()
          
        }

    }
</script>
<style scoped>


/*==================================================
  Post Contents CSS
  ==================================================*/

.post-content{
  background: #f8f8f8;
  border-radius: 4px;
  width: 100%;
  border: 1px solid #f1f2f2;
  margin-bottom: 20px;
  overflow: hidden;
  position: relative;
}

.post-content img.post-image, video.post-video, .google-maps{
  width: 100%;
  height: auto;
}

.post-content .google-maps .map{
  height: 300px;
}

.post-content .post-container{
  padding: 20px;
}

.post-content .post-container .post-detail{
  margin-left: 65px;
  position: relative;
}

.post-content .post-container .post-detail .post-text{
  line-height: 24px;
  margin: 0;
}

.post-content .post-container .post-detail .reaction{
  position: absolute;
  right: 0;
  top: 0;
}

.post-content .post-container .post-detail .post-comment{
  display: inline-flex;
  margin: 10px auto;
  width: 100%;
  flex-direction: column;
}
.post-cont{
  background: #8195ce30;
    padding: 10px;
    border-radius: 10px;
}

.post-content .post-container .post-detail .post-comment img.profile-photo-sm{
  margin-right: 10px;
}

.post-content .post-container .post-detail .post-comment .form-control{
  height: 30px;
  border: 1px solid #ccc;
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075);
  margin: 7px 0;
  min-width: 0;
}

img.profile-photo-md {
    height: 50px;
    width: 50px;
    border-radius: 50%;
}

img.profile-photo-sm {
    height: 40px;
    width: 40px;
    border-radius: 50%;
}

.text-green {
    color: #8dc63f;
}

.text-red {
    color: #ef4136;
}

.following {
    color: #8dc63f;
    font-size: 12px;
    margin-left: 20px;
}
</style>

