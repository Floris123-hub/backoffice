<template>
    <div>
        <div class="d-flex flex-column" 
            style="height: 97vh; width: 52vw; background: #EFF3F6" v-if="dataDiscussion !== null">
            <div class="bg-white
                        d-flex flex-row
                        justify-content-around
                        font-weight-bold py-3"
                style="font-size: 12px; letter-spacing: 1px">
                Chat with {{ dataDiscussion.disc.name }}
            </div>
            <div v-if="dataDiscussion.disc.messages" class="pt-3">
                    <div v-for="(msg, index) in dataDiscussion.disc.messages" :key="index">
                        <div v-if="msg.sender === 'Me'">
                            <div class="ml-auto" style="width:max-content">
                                <div style="position: relative; 
                                            right: 40px; 
                                            top: 47px;
                                            color: #CCCCCC;
                                            font-style: italic;
                                            font-weight: bold;">{{msg.time}}</div>
                                <b-card text-variant="white" 
                                        class="text-left float-left" 
                                        style="border-radius: 25px; 
                                                background: #00BBD4; 
                                                font-weight: bold; 
                                                letter-spacing: 0.5px;
                                                width:250px">
                                    <b-card-text>{{msg.text}}</b-card-text>
                                </b-card>
                                <b-img src="https://cdn.pixabay.com/photo/2016/08/20/05/36/avatar-1606914_960_720.png" 
                                        width="25" 
                                        height="25"
                                        rounded="circle"
                                        alt="Me" class="mt-5 ml-2 mr-3"></b-img>
                            </div>
                        </div>
                        <div v-if="msg.sender === dataDiscussion.disc.name">
                            <div style="width:max-content d-flex flex-row">
                                <b-img src="https://cdn.pixabay.com/photo/2013/07/13/10/07/man-156584_960_720.png" 
                                        width="25" 
                                        height="25"
                                        rounded="circle"
                                        alt="Me" class="float-left mt-5 ml-3 mr-2"></b-img>
                                <b-card class="text-left" 
                                        style="border-radius: 25px; 
                                                font-weight: bold; 
                                                letter-spacing: 0.5px;
                                                width:250px;
                                                text-shadow: 5px 4px 7px rgba(0, 0, 0, 0.42);">
                                    <b-card-text>{{msg.text}}</b-card-text>
                                </b-card>
                                <div style="position: relative; 
                                            left: 310px; 
                                            bottom: 45px;
                                            color: #CCCCCC;
                                            font-style: italic;
                                            font-weight: bold;">{{msg.time}}</div>
                            </div>
                            
                        </div>
                    </div>                
            </div>
            <div v-if="previewImage !== null" class="imageBox border border-danger">
                <i class="far fa-times fa-2x float-right mx-5 my-4 font-weight-light"
                   style="cursor: pointer" title="Close"
                   @click="previewImage = null"></i>
                <div v-if="checkImageExtension(previewImage)" class="imagePreviewWrapper"
                     :style="{ 'background-image': `url(${previewImage})` }">
                </div>
                <video v-if="checkVideoExtension(previewImage)"  class="imagePreviewWrapper" width="350" height="350" controls>
                    <source :src="previewImage" type="video/mp4">
                </video>
            </div>

            <div class="bg-white mt-auto pt-4 px-4">
                <b-form-textarea name="message" 
                                id="txtbox"
                                no-resize 
                                ref="textarea" 
                                v-model="input"
                                cols="85"
                                rows="1"
                                placeholder="Type your message here..."
                                class="px-4 pt-3 pb-0">
                </b-form-textarea>
                <div id="iconbox" class="pb-2">
                    <div class="d-flex flex-row bg-transparent ml-3" id="icon-group">
                        <label for="file">
                            <i class="fas fa-paperclip fa-2x fa-rotate-180 mx-2"></i>
                        </label>
                        <label for="file">
                            <i class="fas fa-film fa-2x mx-2"></i>
                        </label>
                        <span :class="{ 'triggered': showEmojiPicker }" @click.prevent="toggleEmojiPicker">
                            <i id="emojis" class="fas fa-smile fa-2x mx-2"></i>
                        </span>
                    </div>  
                </div>
                <input id="file" ref="fileInput" type="file" name="file" @input="pickFile" hidden>
            </div>
            <picker id="picker" 
                v-show="showEmojiPicker" 
                title="Pick your emoji..." 
                emoji="point_up" 
                @select="addEmoji"
                />
            
    </div>
        {{ showDiscussionById }}
    </div>
    
   
</template>

<script>
    import { Picker } from 'emoji-mart-vue'

    export default {
        name: "Discussion",

        components: { Picker },

        props:{
            id: String,
            value: {
                type: String,
                default: ''
            }
        },

        computed: {
            showDiscussionById(){
                //console.log(this.id)
                let self = this;
                if (self.id !== '' || this.reload === !this.reload){
                    //fonction qui récupère les éléments
                    self.allDiscussions.forEach(element => {
                        //console.log(element)
                        if(element.disc.name === self.id){
                            //console.log(element.disc.name)
                            self.dataDiscussion = element
                        }
                    })
                } return ''
            }
        },

        data(){
            return{
                imageExtensions: ['jpg', 'gif', 'png', 'jpeg'],
                videoExtensions: ['mp4', 'mov', 'wmv', 'flv', 'avi', 'webm', 'mkv'],
                previewImage: null,
                input: '',
                showEmojiPicker: false,
                allDiscussions: [{
                    'disc': {
                        'name': 'Roger',
                        'messages': {
                            1: {
                                'text': 'Hello how are you today? can you please help me?',
                                'sender': 'Roger',
                                'time': '10:25'
                            },
                            2: {
                                'text': 'Hey Roger, if you have any questions, I\'m here to help you.',
                                'sender': 'Me',
                                'time': '10:55'
                            }
                        }
                    }
                },
                {
                    'disc': {
                        'name': 'Arnold',
                        'messages': {
                            1: {
                                'text': 'Hello how are you today? can you please help me?',
                                'sender': 'Arnold',
                                'time': '12:25'
                            },
                            2: {
                                'text': 'Hey Arnold, if you have any questions, I\'m here to help you.',
                                'sender': 'Me',
                                'time': '12:55'
                            }
                        }
                    }
                },
                {
                    'disc': {
                        'name': 'Alexandre',
                        'messages': {
                            1: {
                                'text': 'Hello how are you today? can you please help me?',
                                'sender': 'Alexandre',
                                'time': '13:25'
                            },
                            2: {
                                'text': 'Hey Alexandre, if you have any questions, I\'m here to help you.',
                                'sender': 'Me',
                                'time': '13:55'
                            }
                        }
                    }
                },
                {
                    'disc': {
                        'name': 'George',
                        'messages': {
                            1: {
                                'text': 'Hello how are you today? can you please help me?',
                                'sender': 'George',
                                'time': '14:25'
                            },
                            2: {
                                'text': 'Hey George, if you have any questions, I\'m here to help you.',
                                'sender': 'Me',
                                'time': '14:55'
                            }
                        }
                    }
                },
                {
                    'disc': {
                        'name': 'Jason',
                        'messages': {
                            1: {
                                'text': 'Hello how are you today? can you please help me?',
                                'sender': 'Jason',
                                'time': '15:25'
                            },
                            2: {
                                'text': 'Hey Jason, if you have any questions, I\'m here to help you.',
                                'sender': 'Me',
                                'time': '15:55'
                            }
                        }
                    }
                },
                {
                    'disc': {
                        'name': 'Kenneth',
                        'messages': {
                            1: {
                                'text': 'Hello how are you today? can you please help me?',
                                'sender': 'Kenneth',
                                'time': '16:25'
                            },
                            2: {
                                'text': 'Hey Kenneth, if you have any questions, I\'m here to help you.',
                                'sender': 'Me',
                                'time': '16:55'
                            }
                        }
                    }
                },
                {
                    'disc': {
                        'name': 'Klaus',
                        'messages': {
                            1: {
                                'text': 'Hello how are you today? can you please help me?',
                                'sender': 'Klaus',
                                'time': '17:25'
                            },
                            2: {
                                'text': 'Hey Klaus, if you have any questions, I\'m here to help you.',
                                'sender': 'Me',
                                'time': '17:55'
                            }
                        }
                    }
                }],
                dataDiscussion: null,
                reload: true,
            }
        },

        methods: {
            checkImageExtension(url){
                let dataSplit = url.toString().toLowerCase().split(';')
                let ext = dataSplit[0].split('/').pop()
                return this.imageExtensions.includes(ext)
            },

            checkVideoExtension(url){
                console.log(url.toString().toLowerCase().split(':'))
            },

            pickFile () {
                let input = this.$refs.fileInput
                let file = input.files
                if (file && file[0]) {
                    let reader = new FileReader
                    reader.onload = e => {
                        this.previewImage = e.target.result
                    }
                    reader.readAsDataURL(file[0])
                    this.$emit('input', file[0])
                }
            },

            toggleEmojiPicker () {
                this.showEmojiPicker = !this.showEmojiPicker
            },

            addEmoji (emoji) {
                const textarea = this.$refs.textarea
                const cursorPosition = textarea.selectionEnd
                const start = this.value.substring(0, textarea.selectionStart)
                const end = this.value.substring(textarea.selectionStart)
                const text = start + emoji.native +  end
                this.input += text
                textarea.focus()
                this.$nextTick(() => {
                    textarea.selectionEnd = cursorPosition + emoji.native.length + 1
                })
            },

            sendMessage(disc){
                console.log(disc.messages)
                let message = this.$refs.textarea.$el.value
                let today = new Date()
                console.log(message)
                let time = today.getHours() + ":" + today.getMinutes()
                console.log(time)
                disc.messages.push({
                    'text': message,
                    'sender': 'Me',
                    'time': time
                })
                this.reload = !this.reload
            }
        }
    }
</script>

<style scoped lang="scss">
    #txtbox::placeholder{
        color: #00BBD4;
        opacity: 1;
    }

    #txtbox:-ms-input-placeholder{
        color: #00BBD4;
    }

    #txtbox::-ms-input-placeholder{
        color: #00BBD4;
    }

    #txtbox{
        background: #EFF3F6; 
        border: none;
        font-size: 11px; 
        font-weight: bold;
    }

    #txtbox:focus{
        border-color: inherit;
        -webkit-box-shadow: none;
        box-shadow: none;
    }

    #iconbox{
        background: #EFF3F6; 
    }

    #icon-group{
        width: max-content;
    }

    #icon-group i{
        color: #CECFD0;
        cursor: pointer;
    }
    
    #picker{
        position: absolute;
        right: 0;
        z-index: 1036;
        max-width: 23vw;
        height: 100vh;
    }

    .imagePreviewWrapper {
        position: absolute;
        left: 0;
        right: 0;
        top: 100px;
        bottom: 0;
        width: 350px;
        height: 350px;
        display: block;
        cursor: pointer;
        margin: 0 auto 30px;
        background-size: cover;
        background-position: center center;
        z-index: 1040;
    }

    .imageBox{
        position: absolute;
        height: 100vh;
        width: 100vw;
        left: 0;
        z-index: 1040;backdrop-filter: blur(10px);
        -webkit-backdrop-filter: blur(10px);
    }
</style>