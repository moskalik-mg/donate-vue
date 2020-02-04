<template>
    <div class="wrapper-comment" @click.stop="controlBlock">
        <div class="comment__item" :class="{active : isActive}">
            <img :src="commentItem.avatar" alt="" class="img-avatar">
            <div class="comment__info">
                <div class="user-name">{{commentItem.name}}</div>

                <div class="user-comment" v-if="!inputEdit">{{commentItem.comment}}</div>
    
                <transition name="input-comment" :duration="{ enter: 400, leave: 0 }">
                    <input type="text" 
                    @click.stop v-if="inputEdit" 
                    v-model="newCommentContent" 
                    @keyup.enter="updateComment"
                    class="input-comment">
                </transition>
            </div>
        </div>
        <div class="editDelete" v-if="formEditDelete">
            <button class="btn btn-edit" @click.capture.stop="editComment">Edit</button>
            <button class="btn btn-remove" @click.capture.stop="removeItem">Delete</button>
        </div>
    </div>
</template>

<script>
export default {
    props: ['commentItem', 'index'],
    data(){
        return{
            formEditDelete: false,
            inputEdit: false,
            isActive: false,
            newCommentContent: this.commentItem.comment
        }
    },
    created(){
        document.addEventListener('click', () => this.toggleDropdown());
    },
    methods:{
        removeItem(){
            if(confirm("Do you really want to delete this comment?")){
                this.$emit('deleteComment', this.index)
                this.formEditDelete = false
                this.inputEdit = false
                this.isActive = false
            }
            else{
                return false
            }
        },
        controlBlock(){
            if(this.inputEdit == false){
                this.formEditDelete = !this.formEditDelete
                this.isActive = !this.isActive
            }
            else{
                this.inputEdit = false
                return false
            }
  
        },
        editComment(){
            this.isActive = !this.isActive
            this.formEditDelete = !this.formEditDelete
            this.inputEdit = !this.inputEdit
        },
        updateComment(){
            this.commentItem.comment = this.newCommentContent
            this.inputEdit = !this.inputEdit

        },
        toggleDropdown(){
            this.inputEdit = false
            this.newCommentContent = this.commentItem.comment
        }
    }
}
</script>

<style scoped>
    .wrapper-comment{
        position: relative;
    }

    .comment__item{
    position: relative;
    margin-bottom: 20px;
    border-radius: 5px;
    border: 1px solid #eee;
    padding: 20px;
    display: flex;
    transition: all .4s ease;
    cursor: pointer;
    }

    .comment__item.active{
        filter: blur(2px);

    }

    .comment__info{
    margin-left: 40px;
    }

    .img-avatar{
    width: 70px;
    height: 70px;
    border-radius: 50%;
    }

    .comment__item:hover{
        box-shadow: 0 5px 32px rgba(103,122,141,.17);
    }


    .user-name{
    margin-bottom: 10px;
    }
    .editDelete{
        position: absolute;
        left: 0;
        right: 0;
        top: 0;
        bottom: 0;
        display: flex;
        flex-direction: column;
        align-items: flex-end;
        justify-content: center;
    }
    .btn-edit{
        border: 1px solid #25343b;
        background-color: #fff;
        min-width: 100px;
    }
    .btn-remove{
        background-color: #e53935;
        color: #fff;
        transition: all .4s ease;
        min-width: 100px;
    }

    .btn-remove:hover{
        box-shadow: 0px 10px 35px 0px rgba(229, 56, 53, 0.5);
    }

    .editDelete button:first-child{
        margin-bottom: 10px;
    }


    .input-comment{
        padding: 0;
        font-size: 16px;
    }

    .input-comment-enter-active{
        transition: all .4s ease;
    }

    .input-comment-enter{
        width: 0;
    }

    .input-comment-leave, .input-comment-leave-to, .input-comment-leave-active{
        opacity: 0;
        display: none;
    }
    

    /* .input-comment-leave-to, .comment-leave-to{
        width: 0;
        padding: 0;
    } */

</style>