<template>
    <div>
        <b-sidebar body-class="border"
                   header-class="border border-bottom-0 bg-white"
                   id="sidebar-1"
                   no-header-close
                   visible
                   width="340px">
            <template #header>
                <b-nav-item-dropdown class="w-auto
                                            d-flex
                                            flex-row
                                            mr-1"
                                     no-caret>
                    <template slot="button-content">
                        <strong class="my-0 black" v-if="sortBy!==''">Sorty by {{sortBy}}</strong>
                        <strong class="my-0 black" v-else>Sorty by {{sorters.default}}</strong>
                        <span>
                            <i class="far fa-chevron-down pointer ml-2 black"></i>
                        </span>
                    </template>
                    <span :key="index" v-for="(sorter, index) in sorters">
                        <b-dropdown-item @click="getTypeDisc(sorter)">{{sorter}}</b-dropdown-item>
                    </span>

                </b-nav-item-dropdown>
                <b-input-group class="w-75
                                      d-flex
                                      bg-white
                                      text-dark
                                      align-items-center
                                      border
                                      rounded
                                      pr-3">
                    <b-form-input class="border-0 search"
                                  placeholder="Search...."
                                  type="search" v-model="search" @keyup = "getTypeDisc('search')">
                    </b-form-input>
                    <b-input-group-prepend>
                        <span>
                            <i class="fas fa-search fa-3x" style="color:#DBDFE2;"></i>
                        </span>
                    </b-input-group-prepend>
                </b-input-group>
            </template>
            <div id="enfant">
                <div class="d-flex
                        pl-2
                        flex-row
                        border
                        bg-white
                        border-top-0
                        border-right-0
                        border-left-0
                        justify-content-around py-3">
                <span :key="index" v-for="(group, index) in msgGroup">
                    <div :class="selectIt(index) ? group.elClass+' activer' : group.elClass"
                         :id="group.id"
                         @click="getTypeDisc(group.id), select=index">
                        {{group.text}}<i
                            :class="selectIt(index) ? group.iconClass + ' text-primary' : group.iconClass"></i>
                    </div>
                    <a v-if="group.badgeCount">
                        <b-badge :class="selectIt(index) ? group.badgeClass + ' text-primary font-weight-bold' : group.badgeClass"
                                 style="position:relative;
                                        height:12px;
                                        width: 12px;
                                        color: #C4C4C4;
                                        right: 10px;
                                        top: -11px"
                                 variant="">
                            {{group.badgeCount}}
                        </b-badge>
                    </a>

                </span>

            </div>
            <div>
                <b-list-group>
                    <span :key="index" v-for="(disc, index) in sortMode">
                        <b-list-group-item @click="openDiscussion(disc.conv.name), selected=index"
                                           :class="isSelected(index) ? disc.conv.convClass + ' bg-info text-light border-right-bold ' : disc.conv.convClass + ' border-right-0'">
                            <span class="d-flex flex-row pointer w-100">
                                <div class="d-flex flex-column">
                                    <div class="d-flex flex-row">
                                        <div :style="isSelected(index) ? 'background: #8B8B8B' : 'background:'+disc.conv.background "
                                             class="rounded-circle my-auto conv">A</div>
                                        <b-badge :style="`right: 12px;
                                                          height: 16px;
                                                          width: 16px;
                                                          color: `+disc.conv.iconColor+`;`"
                                                 :variant="isSelected(index) ? 'info' : 'light'"
                                                 class="position-relative
                                                        rounded-circle
                                                        px-0">
                                            <i :class="disc.conv.iconClass"></i>
                                        </b-badge>
                                    </div>
                                </div>

                                <div class="d-flex flex-column w-auto">
                                    <div class="d-flex flex-row ">
                                        <span class="pt-2
                                                     w-75
                                                     d-flex
                                                     justify-content-between">
                                            <strong :class="isSelected(index) ? 'text-light' : 'text-primary'"
                                                    :id="'disc'+index"
                                                    style="font-size: 13px">{{disc.conv.name}}</strong>
                                            <h6 class="mt-1"
                                                style="color: #CECFD0">{{disc.conv.date.day}} {{disc.conv.date.month}}</h6>
                                        </span>
                                    </div>
                                    <span class="w-75">
                                        {{disc.conv.messages}}
                                    </span>
                                </div>

                                <div class="w-auto d-flex flex-column">
                                    <b-badge class="mt-2
                                                    ml-auto
                                                    mb-4
                                                    rounded-circle"
                                             style="height: 15px;
                                                    width: 15px;
                                                    background: #FF7800;
                                                    font-size: 11px">{{disc.conv.msgCount}}</b-badge>
                                    <span class="w-25">
                                        <b-badge class="px-2 py-1"
                                                 style="background: #36354B;
                                                        border-radius: 5px;
                                                        font-size: 9.2px;">{{disc.conv.badge}}</b-badge>
                                    </span>
                                </div>
                            </span>

                        </b-list-group-item>
                    </span>
                </b-list-group>
            </div>
            </div>
            
            <template #footer>
                <div class="d-flex
                            bg-light
                            text-dark
                            px-3
                            py-2
                            border">
                    <div class="w-auto
                                d-flex
                                align-items-center
                                flex-row
                                mx-auto">
                        <strong class="d-flex flex-row">John Doe</strong>
                        <span>
                            <i class="fas fa-chevron-down mx-2" style="color:#A4AFB7;"></i>
                        </span>
                        <div>
                            <b-img alt="profile"
                                   height="50"
                                   rounded="circle"
                                   src="https://cdn.pixabay.com/photo/2016/08/20/05/38/avatar-1606916_960_720.png"
                                   width="50"></b-img>
                            <b-badge class="rounded-circle btn"
                                     id="badge"
                                     ref="status"
                                     style="height: 9px;
                                            width: 2px;
                                            position:relative;
                                            color: transparent;
                                            right: 10px;
                                            bottom: -18px"
                                     variant="success">.
                            </b-badge>
                            <b-tooltip placement="right"
                                       target="badge"
                                       triggers="click"
                                       variant="light shadow rounded border p-0 ml-2">
                                <div :key="index" class="d-flex flex-column" v-for="(statut, index) in statusList">
                                    <div @click.self.prevent="changeStatus"
                                         class="align-items-center
                                                        d-flex
                                                        pointer"
                                         href="">
                                        <b-badge :class="'rounded-circle mr-1 p-0 '+statut.class"
                                                 :variant="statut.color"
                                                 ref="status"
                                                 style="height: 6px;
                                                        width: 6px;
                                                        color: transparent">.
                                        </b-badge>
                                        {{statut.status}}
                                    </div>
                                </div>
                            </b-tooltip>
                        </div>
                    </div>
                </div>
            </template>
        </b-sidebar>
        <div class="w-75 float-right d-flex">
            <discussion :id="id"></discussion>
            <div>
                <b-sidebar body-class="border bg-white"
                           no-header
                           no-header-close
                           right
                           visible
                           width="23vw">
                           <div>
                                <b-alert fade show dismissible id="alert"
                                    class="text-white position-absolute">
                                        <div id="profile" class="mt-0 float-left">
                                            <i class="fas fa-user position-relative"></i>
                                            <b-badge id="badge-alert" class="rounded-circle position-relative">
                                                <i class="fab fa-twitter text-primary"></i>
                                            </b-badge>
                                        </div>
                                        <div id="alert-body" class="position-relative float-right">
                                            <span id="alert-title" class="font-weight-bold ml-3">Visitor 7</span>
                                            <div id="alert-text" class="py-2 pl-2">
                                                Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque pretium
                                            </div>
                                        </div>
                                </b-alert>
                            </div>
                </b-sidebar>
            </div>
        </div>
    </div>
</template>

<script>
    
    import Discussion from "./Discussion";

    export default {
        name: "Conversations",
        components: {Discussion},
        data() {
            return {
                statusList: {
                    1: {
                        'status': 'Online',
                        'color': 'success',
                        'class': ''
                    },
                    2: {
                        'status': 'Busy',
                        'color': 'warning',
                        'class': ''
                    },
                    3: {
                        'status': 'Away',
                        'color': 'light',
                        'class': 'border border-dark'
                    }
                },
                sorters: {
                    'default': 'all',
                    'messenger': 'messenger',
                    'twitter': 'twitter',
                    'web': 'web',
                    'sms': 'sms',
                    'whatsapp': 'whatsapp',
                },
                msgGroup: {
                    'all': {
                        'id': 'all',
                        'elClass': `social-group
                            border-0
                            btn
                            btn-outline-primary
                            rounded-circle
                            px-1
                            py-2`,
                        'iconClass': '',
                        'text': 'Tout',
                        'badgeCount': 7,
                        'badgeClass': `rounded-circle
                                        bg-white mx-0`
                    },
                    'messenger': {
                        'id': 'messenger',
                        'elClass': `social-group
                            border-0
                            btn
                            btn-outline-primary
                            rounded-circle
                            px-1
                            py-2`,
                        'iconClass': 'fab fa-facebook-messenger',
                        'text': '',
                        'badgeCount': 1,
                        'badgeClass': `rounded-circle
                                        bg-white mx-0`
                    },
                    'twitter': {
                        'id': 'twitter',
                        'elClass': `social-group
                            border-0
                            btn
                            btn-outline-primary
                            rounded-circle
                            px-1
                            py-2`,
                        'iconClass': 'fab fa-twitter',
                        'text': '',
                        'badgeCount': 1,
                        'badgeClass': `rounded-circle
                                        bg-white mx-0`
                    },
                    'web': {
                        'id': 'web',
                        'elClass': `social-group
                            border-0
                            btn
                            btn-outline-primary
                            rounded-circle
                            px-1
                            py-2`,
                        'iconClass': 'fas fa-globe-africa',
                        'text': '',
                        'badgeCount': 1,
                        'badgeClass': `rounded-circle
                                        bg-white mx-0`
                    },
                    'sms': {
                        'id': 'sms',
                        'elClass': `social-group
                            border-0
                            btn
                            btn-outline-primary
                            rounded-circle
                            px-1
                            py-2`,
                        'iconClass': 'fas fa-sms',
                        'text': '',
                        'badgeCount': 3,
                        'badgeClass': `rounded-circle
                                        bg-white mx-0`
                    },
                    'whatsapp': {
                        'id': 'whatsapp',
                        'elClass': `social-group
                            border-0
                            btn
                            btn-outline-primary
                            rounded-circle
                            px-1
                            py-2`,
                        'iconClass': 'fab fa-whatsapp',
                        'text': '',
                        'badgeCount': 1,
                        'badgeClass': `rounded-circle
                                        bg-white mx-0`
                    }
                },
                allDisc: [{
                        'conv': {
                            'background': 'purple',
                            'iconColor': 'blue',
                            'iconClass': 'fab fa-facebook-messenger',
                            'name': 'Roger',
                            'iconLetter': 'R',
                            'date': {
                                'day': 30,
                                'month': 'Jul'
                            },
                            'msgCount': 5,
                            'messages': 'Hello how are you today? can you please help me?',
                            'badge': 'MoMo',
                            'canal': 'messenger',
                            'convClass': `border-top-0 
                                          border-left-0
                                          pb-4`
                        }
                    },
                    {
                        'conv': {
                            'background': '#0084FF',
                            'iconColor': '#0084FF',
                            'iconClass': 'fab fa-twitter',
                            'name': 'Arnold',
                            'iconLetter': 'A',
                            'date': {
                                'day': 20,
                                'month': 'Jul'
                            },
                            'msgCount': 9,
                            'messages': 'Hello how are you today? can you please help me?',
                            'badge': 'My Mtn',
                            'canal': 'twitter',
                            'convClass': `border-top-0 
                                          border-left-0
                                          pb-4`
                        }
                    },
                    {
                        'conv': {
                            'background': '#FF7800',
                            'iconColor': '#00B227',
                            'iconClass': 'fab fa-whatsapp',
                            'name': 'Alexandre',
                            'iconLetter': 'A',
                            'date': {
                                'day': 10,
                                'month': 'Jul'
                            },
                            'msgCount': 7,
                            'messages': 'Hello how are you today? can you please help me?',
                            'badge': 'My Mtn',
                            'canal': 'whatsapp',
                            'convClass': `border-top-0 
                                          border-left-0
                                          pb-4`
                        }
                    },
                    {
                        'conv': {
                            'background': '#E10085',
                            'iconColor': '#FFC53F',
                            'iconClass': 'fas fa-globe-africa',
                            'name': 'George',
                            'iconLetter': 'G',
                            'date': {
                                'day': 15,
                                'month': 'Jul'
                            },
                            'msgCount': 7,
                            'messages': 'Hello how are you today? can you please help me?',
                            'badge': 'My Mtn',
                            'canal': 'web',
                            'convClass': `border-top-0 
                                          border-left-0
                                          pb-4`
                        }
                    },
                    {
                        'conv': {
                            'background': '#FFC53F',
                            'iconColor': '#36354B',
                            'iconClass': 'fas fa-sms',
                            'name': 'Jason',
                            'iconLetter': 'J',
                            'date': {
                                'day': 9,
                                'month': 'Jul'
                            },
                            'msgCount': 4,
                            'messages': 'Hello how are you today? can you please help me?',
                            'badge': 'My Mtn',
                            'canal': 'sms',
                            'convClass': `border-top-0 
                                          border-left-0
                                          pb-4`
                        }
                    },
                    {
                        'conv': {
                            'background': '#00B227',
                            'iconColor': '#FFC53F',
                            'iconClass': 'fas fa-sms',
                            'name': 'Kenneth',
                            'iconLetter': 'K',
                            'date': {
                                'day': 6,
                                'month': 'Jul'
                            },
                            'msgCount': 7,
                            'messages': 'Hello how are you today? can you please help me?',
                            'badge': 'My Mtn',
                            'canal': 'sms',
                            'convClass': `border-top-0 
                                            border-left-0 
                                            pb-4`
                        }
                    },
                    {
                        'conv': {
                            'background': '#00B227',
                            'iconColor': '#FFC53F',
                            'iconClass': 'fas fa-sms',
                            'name': 'Klaus',
                            'iconLetter': 'K',
                            'date': {
                                'day': 9,
                                'month': 'Jul'
                            },
                            'msgCount': 5,
                            'messages': 'Hello how are you today? can you please help me?',
                            'badge': 'My Mtn',
                            'canal': 'sms',
                            'convClass': `border-top-0 
                                          border-left-0
                                          pb-4`
                        }
                    }],
                sortBy: '',
                id: null,
                selected: null,
                select: null,
                search: '',
                type: 'all'
            }
        },

        computed: {
            filteredDiscussions() {
                return this.allDisc.filter(disc => {
                    return disc.conv.name.toString().toLowerCase().includes(this.search.toLowerCase())
                })
            },
            sortMode() {
                let disc = this.allDisc
                switch (this.type) {
                    case 'all':
                        return disc;

                    case 'messenger':
                        return disc.filter(element => {
                            return element.conv.canal === 'messenger'
                        })
                    
                    case 'twitter':
                        return disc.filter(element => {
                            return element.conv.canal === 'twitter'
                        })

                    case 'whatsapp':
                        return disc.filter(element => {
                            return element.conv.canal === 'whatsapp'
                        })
                    
                    case 'sms':
                        return disc.filter(element => {
                            return element.conv.canal === 'sms'
                        })

                    case 'web':
                        return disc.filter(element => {
                            return element.conv.canal === 'web'
                        })

                    case 'search':
                        return disc.filter(element => {
                            return element.conv.name.toLowerCase().includes(this.search.toLowerCase())
                        })
                }
                return 0;
            },
        },

        methods: {
            changeStatus() {
                document.addEventListener('click', function (e) {
                    e = e || window.event;
                    let target = e.target || e.srcElement,
                        text = target.textContent || target.innerText,
                        badge = document.querySelector('#badge');
                    //console.log(text)
                    //console.log(badge.classList[3])
                    if (text.includes('Online')) {
                        badge.classList.remove(badge.classList[3], 'border', 'border-dark')
                        badge.classList.add('badge-success')
                    } else if (text.includes('Busy')) {
                        badge.classList.remove(badge.classList[3], 'border', 'border-dark')
                        badge.classList.add('badge-warning')
                    } else if (text.includes('Away')) {
                        badge.classList.remove(badge.classList[3])
                        badge.classList.add('badge-light', 'border', 'border-dark')
                    }
                }, false);
            },
            openDiscussion(name) {
                this.id = name
            },
            isSelected(i) {
                return i === this.selected
            },
            selectIt(i) {
                return i === this.select
            },
            getTypeDisc(type) {
                this.sortBy = type;
                this.type = type
            }
        }
    }
</script>

<style scoped>
    * {
        font-size: 11px;
    }

    .fa-chevron-down {
        font-size: 15px;
    }

    .search {
        font-size: 11px;
    }

    .search::placeholder {
        color: #DBDFE2;
        opacity: 1;
    }

    .search:-ms-input-placeholder {
        color: #DBDFE2;
    }

    .search::-ms-input-placeholder {
        color: #DBDFE2;
    }

    .black {
        color: #36354B;
    }

    .input-group-text {
        width: 20px;
        border: none;
        background-color: #ffffff;
    }

    .pointer {
        cursor: pointer;
    }

    .social-group {
        background: #C4C4C4;
        height: 33px;
        width: 33px;
        color: #FFFFFF;
        font-size: 12px;
    }

    .conv {
        height: 30px;
        padding: 15% 0 0 22%;
        width: 30px;
        color: white;
    }

    .border-right-bold{
        border-right: 5px solid #008a9c;
    }

    .activer {
            background: #00BBD4;
    }

    #sidebar-1 #enfant {
        ::-webkit-scrollbar {
            width:10px;
        }

        ::-webkit-scrollbar-track {
            background: #f1f1f1;
        }

        ::-webkit-scrollbar-thumb {
            background:#888;
        }

        ::-webkit-scrollbar-thumb:hover{
            background:#555;
        }
    }

    #alert{
        height: 18vh;
        width: 25vw;
        right: 10px;
        bottom: 10px;
        background: #00BBD4;
    }

    #profile{
        height: 30px;
        width: 30px;
        background: #DBDFE2;
        color: #CCCCCC;
        border-radius: 50px;
    }

    #profile i{
       left: 10px;
       top: 8px;
    }

    #badge-alert{
        height: 15px;
        width: 12px;
        top: -2px;
        right: -8px;
        background: #00BBD4;
    }

    #alert-body{
        left: 25px;
        bottom: 20px;
    }

    #alert-title{
        font-size: 13px;
    }

    #alert-text{
        background: #00A3B9;
        font-weight: bolder;
        width: 18vw;
        height: max-content;
        border-radius: 5px;
    }
    
</style>