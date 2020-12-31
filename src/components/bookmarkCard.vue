<template>
     <div  v-for =" bookmark in bookmarkList" :key="bookmark.id" class="bookmark-item">
          <div class="header">
            <h3   v-if="!bookmark.cardState">{{bookmark.title}}</h3>
           <input v-else v-model="newBookmark.title" type="text" class="form-control" />
            <button  v-if="!bookmark.cardState" @click="deleteBookmark(bookmark.id)" class="btn-sm btn-danger">
              Kaydı Sil
            </button>
            <button @click="updateData(bookmark)" v-if="bookmark.cardState" class="btn-sm btn-primary">
              Kaydet
            </button>
          </div>
          <p  v-if="!bookmark.cardState" class="mt-20 mb-20">{{bookmark.description}}</p>
             <input v-else v-model="newBookmark.description" type="text" class="form-control" />
          <div class="d-flex justify-content-between align-items-center">
            <a  v-if="!bookmark.cardState" href="#"> {{bookmark.url}} </a>
              <input v-else v-model="newBookmark.url" type="text" class="form-control" />
            <button  v-if ="!bookmark.cardState" @click="updateItem(bookmark)" class="btn-warning btn-sm">
              Düzenle
            </button>
             <button  v-else @click="cancelUpdateItem(bookmark)" class="btn-danger btn-sm">
              İptal
            </button>
          </div>
        </div>
</template>

<script>
export default {
  props: ["bookmarkList"],
  data(){
    return{
      newBookmark: {
        title:"",
        description:"",
        url:"",
      },
    }
  },
  methods:{
    deleteBookmark(id){
            this.$emit("emitDeleteBookmark", id);
    },
  updateItem(bookmark_item){
    this.bookmarkList.map((bookmark) => {
        if (bookmark.id !== bookmark_item.id) {
          bookmark.cardState = false;
        }                 
      });
      bookmark_item.cardState = true;
          this.newBookmark = {
            id: bookmark_item.id,
            title: bookmark_item.title,
            url: bookmark_item.url,
            description: bookmark_item.description,
            cardState: false,
          }
  },
  updateData(bookmark){   
    this.$emit("emitNewBookmark",this.newBookmark);
  },
  cancelUpdateItem(bookmark){
         bookmark.cardState=false;
  }
  }
}
</script>