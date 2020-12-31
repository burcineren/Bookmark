<template>
  <div>
    <header>Bookmark App</header>
    <!-- COMPONENT -->
    <addBookmark @emitSaveBookmark="saveData"/>
    <!-- /COMPONENT -->
    <!-- COMPONENT -->
    <bookmarkDetail>
    <bookmarkCard :bookmarkList ="bookmarkList"  @emitDeleteBookmark="deleteData" @emitNewBookmark="updateData" />
    </bookmarkDetail>
    <!-- /COMPONENT -->
  </div>
</template>

<script>
import axios from "axios";
import addBookmark from "@/components/addBookmark";
import bookmarkDetail from "@/components/bookmarkDetail";
import bookmarkCard from "@/components/bookmarkCard";

export default {
  components :{

    addBookmark,bookmarkDetail,bookmarkCard,
  },
  data(){
    return {
      bookmarkList: [],   
    }
  },
  methods: {
  saveData(bookmark) {
      axios.post("http://localhost:3000/bookmarks", bookmark).then(bookmark_save_response => {
        console.log("bookmark_save_response", bookmark_save_response);
        // Created...
        if (bookmark_save_response.status == 201) {
       
          this.bookmarkList.push(bookmark_save_response.data);
        }    
      });
    },
  deleteData(id) {
      axios.delete(`http://localhost:3000/bookmarks/${id}`).then(bookmark_delete_response => {
        console.log("bookmark_delete_response", bookmark_delete_response);
        this.bookmarkList = this.bookmarkList.filter(b => b.id !== id);
      });
    },  
  updateData(newbookmark){
   axios.patch(`http://localhost:3000/bookmarks/${newbookmark.id}`, newbookmark).then(update_response => {
        const matchedBookmark = this.bookmarkList.findIndex(b => b.id === newbookmark.id);
        if (matchedBookmark > -1) {
          this.bookmarkList[matchedBookmark] = {
            ...newbookmark
          };   
        }
      });
  },
  },
 created() {
    axios
      .get("http://localhost:3000/bookmarks")
      .then(bookmark_response => {
        console.log("bookmark_response ", bookmark_response);
        this.bookmarkList = bookmark_response.data;
      })
      .catch(e => {
        console.log("Error", e);
      });
  }, 
}
</script>