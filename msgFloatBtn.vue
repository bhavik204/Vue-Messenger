<template>
	<div>
        <div v-if="msgToggle == true">
            <div @click.native="msgPopup" class="speech-bubble-ds btn-pos1">
                <div class="img-pos">
                    <image class="img-ico1" src="https://www.materialui.co/materialIcons/navigation/menu_white_192x192.png"/>
                </div>
                <div class="speech-bubble-ds-arrow" :style="{'border-top': this.floatArrow}"></div>
            </div>
        </div>
        <div v-if="msgPop == true">
            <popup @toggleEvent="closePopup">
                <!-- Step 1 -->
                <div v-if="step1==true" class="pop-layout">
                    <div class="welcome-background">
                        <div class="waveWrapper waveAnimation">
                            <div class="waveWrapperInner bgTop" :style="{'background-color': bgColor}">
                                <div class="wave waveTop" style="background-image: url('http://front-end-noobs.com/jecko/img/wave-top.png')"></div>
                                <v-text class="header-font">{{title}}</v-text>
                                <div class="sub-header-font-align">
                                    <v-text class="sub-header-font">{{subTitle}}</v-text>
                                </div>
                                <!-- <div class="">
                                    <v-text class="sub-header-font">Conversation below.</v-text>
                                </div> -->
                            </div>
                            <div class="waveWrapperInner bgMiddle">
                                <div class="wave waveMiddle" style="background-image: url('http://front-end-noobs.com/jecko/img/wave-mid.png')"></div>
                            </div>
                            <div class="waveWrapperInner bgBottom">
                                <div class="wave waveBottom" style="background-image: url('http://front-end-noobs.com/jecko/img/wave-bot.png')"></div>
                            </div>
                        </div>
                    </div>
                    <div class="second-half">
                        <div class="img-layout">
                            <div class="">
                                <image class="img-size" src="https://img.kpopmap.com/2017/11/shin-jungmin.jpg"/>
                            </div>
                            <div class="">
                                <image class="img-size" src="https://img.kpopmap.com/2017/11/shin-jungmin.jpg"/>
                            </div>
                            <div class="">
                                <image class="img-size" src="https://img.kpopmap.com/2017/11/shin-jungmin.jpg"/>
                            </div>
                        </div>
                        <div class="align-sub-header-font-2">
                            <v-text class="sub-header-font-2">{{details}}</v-text>
                        </div>
                        <div class="msg-btn-align">
                            <div class="vx-primary-btn" @click.native="forwardStep" :style="{ backgroundColor:bgColor}">
                                <text class="vx-primary-btn-label" :style="{fontSize: labelSize}">Start a Conversation</text>
                                <div class="vx-primary-btn-margin-left">
                                    <image :src="'https://www.materialui.co/materialIcons/content/send_white_36x36.png'" class="vx-primary-btn-icon-size" />
                                </div>
                            </div>
                        </div>
                        <div class="richpanel">
                            <div class="rich-align">
                                <v-text class="richpanel2">RichPanel </v-text>
                            </div>
                            <image class="vx-primary-btn-icon-size" src="data:image/png;base64,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"/>
                            <div class="rich-align">
                                <v-text class="richpanel1">Powered by </v-text>
                            </div>
                        </div>
                        <!-- <div class="msg-btn-align">
                            <msg-btn 
                            btn-name="Start a Conversation"
                            btn-color= this.btnColor 
                            icon-side="right" 
                            btn-icon="https://www.materialui.co/materialIcons/content/send_white_36x36.png"
                            @click.native="forwardStep"></msg-btn>
                            
                        </div> -->
                    </div>
                </div>
                <!-- Step 2 -->
                <div v-if="step2==true" class="pop-layout">
                    <div class="welcome-background">
                        <div class="waveWrapper waveAnimation">
                            <div class="waveWrapperInner-2 bgTop1" :style="{ backgroundColor:bgColor}">
                                <div class="back-btn-layout">
                                    <div class="back-btn cursor">
                                        <image @click.native="stepback1" class="back-icon" src="https://www.materialui.co/materialIcons/navigation/arrow_back_white_36x36.png"/>
                                    </div>
                                    <div class="back-btn-title">
                                        <v-text class="back-title-1">Twiks Travels</v-text>
                                        <v-text class="back-title-2">Welcome to omega Support</v-text>
                                    </div>
                                </div>
                                <div class="img-layout-2">
                                    <div class="img-user-align">
                                        <image class="img-size" src="https://img.kpopmap.com/2017/11/shin-jungmin.jpg"/>
                                        <div class="user-name-align">
                                            <v-text class="user-name">Lawrence</v-text>
                                        </div>
                                    </div>
                                    <div class="img-user-align">
                                        <image class="img-size" src="https://img.kpopmap.com/2017/11/shin-jungmin.jpg"/>
                                        <div class="user-name-align">
                                            <v-text class="user-name">Kathrene</v-text>
                                        </div>
                                    </div>
                                    <div class="img-user-align">
                                        <image class="img-size" src="https://img.kpopmap.com/2017/11/shin-jungmin.jpg"/>
                                        <div class="user-name-align">
                                            <v-text class="user-name">Peter</v-text>
                                        </div>
                                    </div>
                                </div>
                                <div class="sub-header-font-align">
                                    <v-text class="sub-header-font">{{details}}</v-text>
                                </div>
                                <!-- <div class="wave waveTop" style="background-image: url('http://front-end-noobs.com/jecko/img/wave-top.png')"></div>
                                <v-text class="header-font">Hello!</v-text>
                                <div class="sub-header-font-align">
                                    <v-text class="sub-header-font">Welcome to our support. Start a new</v-text>
                                </div>
                                <div class="">
                                    <v-text class="sub-header-font">Conversation below.</v-text>
                                </div> -->
                            </div>
                            <div class="waveWrapperInner bgMiddle">
                                <div class="wave waveMiddle" style="background-image: url('http://front-end-noobs.com/jecko/img/wave-mid.png')"></div>
                            </div>
                            <div class="waveWrapperInner bgBottom">
                                <div class="wave waveBottom" style="background-image: url('http://front-end-noobs.com/jecko/img/wave-bot.png')"></div>
                            </div>
                        </div>
                    </div>
                    <div class="chat-box1">
                        <div class="email-layout">
                            <div class="">
                                <v-text class="bot-msg-text">Enter E-mail</v-text>
                            </div>
                            <div class="email-input-align">
                                <input placeholder="Enter EmailID.." type="text" v-model="emailId" class="email-input">
                            </div>
                        </div>
                    </div>
                    <!-- <div class="second-half"> -->
                        <div class="input-box-layout">
                            <input  class="vx-text-field"
                            placeholder="Start a new Conversation" 
                            v-model="userMsg" 
                            v-on:keydown="processEvent"
                            @change="processEvent">
                            <div v-if="attachOn == true" class="attach-icon-align">
                                <image class="attach-icon" src="https://www.materialui.co/materialIcons/editor/attach_file_grey_72x72.png"/>
                            </div>
                            <div class="attach-icon-align">
                                <image class="attach-icon" src="https://www.materialui.co/materialIcons/social/mood_grey_96x96.png"/>
                            </div>
                            <div v-if="sendOn == true" @click.native="forwardStep3" class="attach-icon-align">
                                <image class="attach-icon" src="https://www.materialui.co/materialIcons/content/send_grey_96x96.png"/>
                            </div>
                        </div>
                        <div class="richpanel-2">
                            <div class="rich-align">
                                <v-text class="richpanel2">RichPanel </v-text>
                            </div>
                            <image class="vx-primary-btn-icon-size" src="data:image/png;base64,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"/>
                            <div class="rich-align">
                                <v-text class="richpanel1">Powered by </v-text>
                            </div>
                        </div>
                    <!-- </div> -->
                </div>
                
                <!-- Step 3 -->
                <div v-if="step3==true" class="pop-layout">
                    <div class="welcome-background3" :style="{ backgroundColor:bgColor}">
                        <div class="back-btn-layout">
                            <div class="back-btn cursor">
                                <image @click.native="stepback2"  class="back-icon" src="https://www.materialui.co/materialIcons/navigation/arrow_back_white_36x36.png"/>
                            </div>
                            <div class="img-user-align single-user-align">
                                <image class="img-size" src="https://img.kpopmap.com/2017/11/shin-jungmin.jpg"/>
                            </div>
                            <div class="back-btn-title single-user-title">
                                <v-text class="back-title-1">Peter</v-text>
                                <!-- <v-text class="back-title-2">Welcome to omega Support</v-text> -->
                            </div>
                            
                        </div>
                    </div>
                    <div class="chat-box">
                        <div class="bot-msg">
                            <v-text class="bot-msg-text">Hello {{emailId}}</v-text>
                        </div>
                        <div class="bot-msg">
                            <v-text class="bot-msg-text">Welcome back to our site. What can we do for you today.</v-text>
                        </div>
                        <div class="msg-time">
                            <v-text class="bot-msg-text">12:53 PM</v-text>                        
                        </div>
                        <div v-for="msg in messageList" class="align-user-msg">
                            <div class="sub-align-user-msg">
                                <div class="user-msg">
                                    <v-text class="user-msg-text">{{msg}}</v-text>
                                </div>
                            </div>
                        </div>
                    </div>
                    <!-- <div class="second-half"> -->
                        <div class="input-box-layout">
                            <input  class="vx-text-field"
                            placeholder="Start a new Conversation" 
                            v-model="userMsg" 
                            @change="">
                            <!-- <div  class="attach-icon-align">
                                <image class="attach-icon" src="https://www.materialui.co/materialIcons/editor/attach_file_grey_72x72.png"/>
                            </div> -->
                            <!-- <div class="attach-icon-align">
                                <image class="attach-icon" src="https://www.materialui.co/materialIcons/social/mood_grey_96x96.png"/>
                            </div> -->
                            <div @click.native="sendMessage" @keyup.enter="sendMessage" class="attach-icon-align-send">
                                <image class="attach-icon" src="https://www.materialui.co/materialIcons/content/send_grey_96x96.png"/>
                            </div>
                        </div>
                        <div class="richpanel">
                            <div class="rich-align">
                                <v-text class="richpanel2">RichPanel </v-text>
                            </div>
                            <image class="vx-primary-btn-icon-size" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAOEAAADhCAMAAAAJbSJIAAAAkFBMVEUATpb///8AOo35+vsATJUASpQAQ5EARZIAQZAAR5MAS5UAQJAAPo/s8vcAOY0API4AU5rR3Om4yd3m7fTc5e+Ysc7B0OHz9/pGdKuGocRSfK/q8fc7bKakutSRq8qOqMkyZqN0lL0ZWZxgh7bI1eWtwNdki7hOeq56mL8kYaFmhbM8cKlxi7bW3+oSV5s0aKUyz6nWAAAMoUlEQVR4nO2d6ZqiOhCGaQxJWFwQFHHfW3pG5/7v7mCv2YBgFyDn4ft3zrSY12yVqkphvPzfZTTdgMrVEbZfHWH71RG2Xx1h+9URtl8dYfvVEbZfHWH7VUTYe379htCPZyNvYD6zBt5oFvsPEo6XHrGQ8exCFvGW4wcI+3NqNd14bVl03i9L6Bt2080uJdvIGqoZhL7nNN3mknLcDEQ1Yd9oG2CKaKgHqppw3q4h+iF7rk84pk239iG5yhVVSbhszyrKylrqEvpe0219UJ5qsVERxqTppj4oEmsSzto5SNNhOtMj7I2e31RTC40UZriKsK3T0EBUk3DQdEsf1kCT0Gy6oQ/L7Ag7wqdXR9gRPr86wo7w+dURdoTPr+YJkWWnqi5U0DChTc3b6+4SXw5bz8OVOGSbJLSosxv/uGz9dUIr8Mk2R4ioHB3yzxY4Y2OEeKv01YZnF3hGNkXoxllx2ilwhKsZQmd4zOBL1V9iyO9qhNAahdmAqQ6Q7ucmCNEtHzBFBOzFJgit3CyCdy3h5mIDhO6qEPClD2fj1E+YEbQUNAZzstdPSOTAcxTJrThAjdPaCfFaePJ092d/275NBPAQapzWTYiE3IFp4qXnCoRsagg2AFQn1k1oX7jnXtzvuCTC24D9p7EL8411E9Ir+9gzR2E5bP/6QHtizYROwn7fZMj/q82mFRyBkj9qJrTPzEMXEgNltsoNkOlWMyGeMA+NpXHIdHG0b+NaiqwBe6YwZAay+/rHM5T1XSMhssnyxHzdVbWU0Pn7ghrMwWzv2giRbZ95i3uqXEps8hbHb6R9lrft7gLhmRmZcnfvImvPoF8aN/UQWoO5yJfVh4KImcz+DekvIOsgRHSpOhGGxVMN2evo/qer0eOjtgZCG03kB951K+oZa//lDOg/7oKrnBC5igH6oU1RJ7LOgLH9YGZd1YTOMKMDU0UFhHxaYZg8NlIrJrRznU4nwS41kOP8DEZk8Z/tbx8yAqolxPMoBzC127jZZZPkX/Lj1ncSsRHzR4zxSgndi/wkXmvmfOge/LQx4eWL2nqV/nz3wJmxSkJzLT9IlD8zcXrGt2xv9HWuOH1iWH/kP7+UR6yOEJnZawyr62Z2u/3bMXGazzVWmftavhcrI0RuBmCYtXl8K/pMSMaq6NS5rEleGSFVD9HFxVQlXvN6+5icKFGtU7OSm0ZVhHSnbPvEIIgWjt7dJwRWPSRKym39FRHayo4Kl+/rJJkWEL59dRM+KJoXluvEagjRSDW+Jvhz9BWEZhgHBtkrZu2p1GpTDSG+Kh5x8L4a7tg5AdL0l2A2dkv1p6U8HJUQqmZasGUWQeSesgGnhDV0EJFX1KjwVFIxoeqayvXGzx66y7Ln1oRvvgpxXKI5FRAiLFvbPhIXQHukDCP6f7DYP2goT9sSl5QqIMQbud1UTndC1FiJ/TidmYqmIyL9ZKGnPU7hCZH88cBWtgdRcliF/Y8WRMH4cvPUXWPtpZjjRXuxgSeUF5HsPRrZ1L0t529vb7MEu+rf4S45bhxo58CBE0qnupeX19wtGll392F6vsj7I9nI3enu++CEchfGAEEkNBSnYpjd47ygCfnw2V1T0VXxkKyt2KS55nIKTSiG6cHuZlJxc9FyKBsV9KE4mnZAMSTZ1FWErlQCJpR8KyFYMqW0zV701hpgQipaWH/gbkg7C/7RR70JDkuIbGEo6U4WHUmdqLclwhJKNjfkJXfJM3XWGqawhFhY8I6gV8DFGhcrrQECSyjuy2DJae8SHaih1kQEnof8pwLgW/wDfq3RS9cAJbQEA3kNXExDnARaZg0ooWjQ7IEvwYgLmdaOCEooLAUh/DUUfjPSWmogCRHl58kK9FbBXZR3vB113IqQhM6Wf9greMUXzB8TQ6Kx1EASCgtNBRVf7AP3DcFI4zOQhPZf7jNX3TOqvhBf9UlruwAl5EO+FVQHQzfOJdXbaizWkISEN40Lk0nKC9m80bSsuQ8FQj3DuJw8PiCiU2INdJTuSv/AZeXygTmdowvofrhnH3aV3PMAorwnr25C/kZTJXUWBcK6R2k6E3+myQTougQvwfaue6W5V9P8skw30Nd5P8T7FKOk5t3CuEfWZiffn24S+J3CuOcVDbhR2q97x3+XhTGmBL52gE3JaLaLuf0w0PlgxbmJFsEY4BIawsZ8LOcsaGQZV01Il/FqPf9t4j3C+7Uykcqv+/QkiXzMmuvO/E03Wt4mI+Zf+wlY1M8FGT953GODk8wk3I1Oqyok5I4auwcvFKCM9LEPxRpPBSS8XwVhv5B3nk4eQ3TltAdWq+KMBThCG8/+vu2Zjd7hH716JFLq5QOmP1yhSxaM8ONKVm88+jEVhUevy/uH8VluiqBL0WoDRfidKBl8O0mHYopI6Qt36lLVgoryTYEIGV/t8csBRsT8iSgpORWtovoZd4UF5wsYQmQziVnfQVFLSnkqZ61iuVB1FMlbo3wVtQJC7nS//vpG6584TnXzJ94lRe7DzSxJkteN0LNR/kNhCF32S0/fc98abfjHX8sUXBbcPv0DIZaDHIuQM4+eX00dhJDPT2AuYSOMecdKmbD+gOM4Dr83W2TzpeQXufsQCCEfkeFcC8jkzqwlCiXwHnQ+7wjxAYxcdw0EIfLYh0yFMcO5xyKdSMOHOJdMKKS98a7hSd4whSDkwnqRGDREe3a06a813HYqhcsJa/Re82wJCEJuSZjIhRLYuOlad8NAbI6j/O4JRJhIXpTnUoAgZMdTbysNQ7RnHjTVnYjWG/MpxWrJet16/3LGPgQhu7KpkhPMa/6/K8WdvRRLMLcF5419CMIh42JQLZaY6eOFrteGHfo9xb9zqSdvObYpBKHJrAknhanPGqh9Pb70Q8zs7SnOllwu8t+KCdk+LCTUCdu+f4gxSlXR5Fr7cMjMM19hX3Cj1NEdpew8VHh+uXmYlzEAQciaNJHi1zQZq1V7pWFNmr7iZ2PzyXt53n0IQi5DQg45PbZbIPwzu1VpKw5jR0R5gTwIQi5DQc5x4e555RpY/FO/8y4ixWcoe3bM/dlA7NIR+6eigWVxd13z1gRB3idEsJVnGf/Q3OMTyNmCXWpe+nuuQYiwx4BeGT8GXZ7C4BobMiByuHh+bv4cCKFQwIItteJ43MlKexq+y6IEUcVpxHa486Fq/QYmFC5z9V8/b6Ah5I74m3VlQ9/8oen+XwiRwYH3juTn6cJ4McTrHsdXh7qUWonw/4PfvDYKYWs0MkbbWPDThPnjAsjXthddYIvjeHyUnIHnXwT3rdsk7EWL0nVAgfylyjfUyVoc9G9GioDbjFoM44ITJxAhIsW1gd81TR5jRCQDsH8riKhDefUtaZxm6XSjhScoZGOKuUBW5iDJv9wISGjYilorGRrPSF5iJrLo6Lwan84jxhhTvoryRacQCFx0DR/kj2XJv+y9DMh0D5ytPnaDKP5BNNVDRKOYC2CElBSUTOIUHePt0MXs5ViUjk3Pmk+YCffj1jIXqoccNAL5oLmJ2RF3pfrj9XzvDYae53pDc4C3u5WfdbVJuhSX6qpV/Qs0jo8GGnWTREWhfzz6V/VSufiy2eSSStFloLUqA2cq4OSkeOAv9H16F2ptBGtD8xwGfkvWTfjoVzSdF8Xi8zT5WisRjr+neTA+ONrZq/DZJg6xt7vTMQwWgT+dzG+uTV9LLEGCfo73CN8O69VqEh+2Rpn30VSRT4Mcm2JiE4Iped/cbSO3Hk2e2MNtutbeDQG7XFpghdXM2I3AuzzYjXKQoKzqqkGLk6LiUEoBFPWur44wPRQWapO/VuGieVrCdDZame8HylD/D0A2fK31vMkwK49Spd4aQ1x+q7cmO8L2RdOyCzYGzL2p2t/gQfB8XNiRwepgQV1Iqf8tLOnx73bOgwxO87Svwb6vkTdaIZuSP+upgnIxjhMMMv2+1dR715CFXXM/jydjPwz6/eB6PE3i15HpkvxySuXV3NsBjTtmati5w6FpDocuJdW8yLJRwlrUEXaEz6+OsCN8fnWEHeHzqyPsCJ9fuoS/SaJoVgNNQuDKa/UJUU1C+IpINUn5HhAFIWiJx1qlKhCvJMy/PfXEUiZxqAjlOx0tkTKJQ0WoVQrmCaW+aKskrKByVx0Sa57mEFZTM6hqZbzmXE3YR/AlWaqWY8ivOc8mTBebtiE6WbliGYQvvtGugWobitfd5BK+9Oe0PSuqRefqIZpHmK6oS49U4nuHFbKIt1SuooWE6VCNZyNvYD6zBt5oFudm9OYSpuo9vwoIigjbr46w/eoI26+OsP3qCNuvjrD96gjbr46w/eoI26//P+F/cB7ddvJqJQIAAAAASUVORK5CYII="/>
                            <div class="rich-align">
                                <v-text class="richpanel1">Powered by </v-text>
                            </div>
                        </div>
                    <!-- </div> -->
                </div>
            </popup>
        </div>
	</div>
</template>
<script>
    const animation = weex.requireModule('animation')
    const env = weex.config.env || WXEnvironment
    const invalid = '- invalid -'
    import popup from './msgpopup.vue'
    import msgBtn from './msgBtn.vue'
    // import popup from './'
    export default {
        
        components:{
            popup,
            msgBtn
        },
        props: {
            bgColor: {
                type: String,
                default: '#86377b'
            },
            title:{
                type: String,
                default: 'Hello!'
            },
            subTitle:{
                type: String,
                default: 'Welcome to our support. Start a new Conversation here.'
            },
            details:{
                type: String,
                default: 'The team typically replies in few minutws.'
            },
        },
        created() {
            if (typeof WXEnvironment === 'object') {
                this.platform = WXEnvironment.platform || unknown
                return this.platform
            }
            this.btnColor = this.bgColor
            console.log('oppa',this.btnColor)
        },
        created(){
            this.floatArrow += this.bgColor
            this.btnColor = this.bgColor
            console.log('oppa',this.floatArrow)
            if(this.step2 == false || this.step3 == false){
                setTimeout(this.msgPopup, 10000)
            }
            // setTimeout(function(){ alert("Hello"); }, 3000);
            // setTimeout(function(){ 
            //     this.msgPopup()
            //     console.log('oppakugugu',this.msgPop,this.step1)  }, 3000);
            // console.log('oppa',this.bgColor)
            
        },
        data() {
            return {
                btnColor:'',
                emailId:'',
                step1: true,
                step2: false,
                step3: false,
                msgToggle:true,
                msgPop: false,
                attachOn: true,
                sendOn: false,
                messageList:[],
                userMsg:"",
                floatArrow: "13px solid" 

            }
        },

        methods: {
            msgPopup(){
                this.msgToggle = false  
                this.msgPop=true
                this.step1 = true
                console.log(this.msgPop)
            },
            closePopup(e){
                console.log('lets see',e)
                this.msgPop = e  
                if(e == false){
                    this.msgToggle = true
                }
                console.log('lets',this.msgToggle)
                this.step1 = false
                this.step2 = false
                this.step3 = false
                this.attachOn= true
                this.sendOn = false
                // this.msgPop=msgPopOff
            },
            forwardStep(){
                this.step1 = false
                this.step2 = true
            },
            forwardStep3(){
                this.step3 = true
                this.step2 = false
                this.sendMessage()
            },
            stepback1(){
                this.step1 = true
                this.step2 = false
                console.log(this.step2,this.step1)
            },
            stepback2(){
                this.step3 = false
                this.step2 = true
            },
            processEvent(){
                this.attachOn = false
                this.sendOn = true
            },
            sendMessage(){
                this.messageList.push(this.userMsg)
                this.userMsg=""
            }
        }
    }
</script>
<style>
    
    /*==========  Mobile First Method  ==========*/
    /* Custom, iPhone Retina */
    
    @media only screen and (min-width: 320px) {
        .richpanel{
            flex-direction: row-reverse;
            padding-right: 10px;
            padding-top: 15px;
        }
        .richpanel-2{
            flex-direction: row-reverse;
            padding-right: 10px;
            padding-top: 0px;
        }
        .rich-align{
            padding-top: 2px
        }
        .richpanel1{
            font-size : 8px;
            /* font-weight: 500; */
            color: rgb(88, 83, 83);
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
        }
        .richpanel2{
            font-size : 8px;
            /* font-weight: 500; */
            color: rgb(91, 188, 233);
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
        }
        /* Button */
        .mobile{
            display: block
        }
        .web{
            display: none
        }
        .vx-primary-btn {
            border-radius: 6px;
            height: 25px;
            width: 140px;
            position: relative;
            cursor: pointer;
            flex-direction: row;
            padding-top: 6px;
            padding-left: 12px;
        }
        .vx-primary-btn-label{
            font-size : 10px;
            font-weight: 500;
            color: white;
            cursor: pointer;
        }
        .vx-primary-btn-margin-right {
            margin-right: 10px;
        }
        .vx-primary-btn-margin-left {
            margin-left: 10px;
        }

        .vx-primary-btn-icon-size {
            width: 15px;
            height: 15px;
            cursor: pointer;
        }
        /* ====== */
        .speech-bubble-ds {
            background-color: #00aabb;
            width: 35px;
            height: 35px;
            border-radius: 8px;
            padding: 15px;
            position: fixed;
        }

        .speech-bubble-ds p {
            margin-bottom: 10px;
        }
        .speech-bubble-ds p:last-of-type {
            margin-bottom: 0;
        }

        .speech-bubble-ds-arrow {
            border-left: 21px solid transparent;
            /* border-top: 20px solid rgba(0, 0, 0, 0.2); */
            bottom: -25px;
            position: absolute;
            right: 15px;
        }
        .speech-bubble-ds-arrow::before {
            border-left: 15px solid transparent;
            border-top: 13px solid #00aabb;
            bottom: 18px;
            content: "";
            position: absolute;
            right: -15px;
        }
        .speech-bubble-ds-arrow::after {
            /* border-left: 21px solid transparent;
            border-top: 21px solid #efefef;
            bottom: 4px;
            content: "";
            position: absolute;
            right: 6px; */
        }
        .img-pos{
            align-items: center
        }
        .btn-fab {
            padding: 13px;
        }
        .img-ico1 {
            width: 30px;
            height: 30px;
        }
        .img-ico2 {
            width: 25px;
            height: 25px;
        }
        .btn-pos1 {
            bottom: 75px;
            right: 25px;
            justify-content: center;
            cursor: pointer
            /*background-color: #ddd;*/
        }
        .btn-pos2 {
            bottom: 145px;
            right: 34px;
            /*background-color: #2196f3;*/
            padding: 7px;
        }
        .btn-pos3 {
            bottom: 195px;
            padding: 7px;
            right: 34px;
            /*background-color: #2196f3;*/
        }
        .pop-layout{
            flex-direction: column;

        }
        .welcome-background{
            border-top-right-radius: 6px;
            border-top-left-radius: 6px;
            /* padding-top: 30px; */
            height: 145px;
            /* background-color: #00aabb */
        }
        .welcome-background3{
            border-top-right-radius: 6px;
            border-top-left-radius: 6px;
            padding-top: 10px;
            height: 45px;
            /* background-color: #86377b */
        }
        .header-font{
            color: white;
            text-align: center;
            font-size: 24px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
        }
        .sub-header-font-align{
            margin-left: 15px;
            margin-right: 15px;
            margin-top: 10px
        }
        .sub-header-font{
            color: white;
            text-align: center;
            font-size: 10px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
        }
        .second-half{
            padding-top: 30px
        }
        .img-layout{
            flex-direction: row;
            justify-content: center
        }
        .img-layout-2{
            flex-direction: row;
            justify-content: space-around;
            margin-top: 20px;
            padding-left: 25px;
            padding-right: 25px;
        }
        .img-size{
            height: 25px;
            width: 25px;
            border-width: 0.5px;
            border-color: lightgrey;
            background-color: blueviolet;
            border-radius: 50%;
        }
        .align-sub-header-font-2{
            margin-top: 8px
        }
        .sub-header-font-2{
            text-align: center;
            color:darkgray; 
            font-size: 10px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
        }
        .msg-btn-align{
            justify-content: center;
            flex-direction: row;
            margin-top: 20px;
        }
        .back-btn{
            margin-top: 3px
        }
        .back-btn-layout{
            flex-direction: row;
            margin-left: 10px
        }
        .back-icon{
            cursor: pointer;
            width: 18px;
            height: 18px;
        }
        .back-btn-title{
            margin-left: 10px
        }
        .back-title-1{
            color:#ffffff; 
            font-size: 12px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
        }
        .back-title-2{
            color:#ffffff; 
            font-size: 10px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
        }
        .user-name{
            color:#ffffff; 
            text-align: center;
            font-size: 8px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
        }
        .user-name-align{
            margin-top: 5px
        }
        .img-user-align{
            align-items: center
        }
        .input-box-layout{
            flex-direction: row;
            margin-top: 0px;
            border-width: 1px;
            border-style: solid;
            border-top-color: darkgray;
            border-left-color: #FFF;
            border-right-color: #FFF;
            border-bottom-color: #FFF;
        }
        .vx-text-field {
             padding: 10px; 
            /* background-color: green; */
            /*padding-top: 25px;*/
            font-size: 10px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
            width: 80%;
            border-width: 2px;
            border-style: solid;
            border-top-color: #FFF;
            border-left-color: #FFF;
            border-right-color: #FFF;
            border-bottom-color: #FFF;
        }
        .email-input{
            padding: 10px; 
            /* background-color: green; */
            /*padding-top: 25px;*/
            font-size: 10px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
            width: 65%;
            height: 25px;
            border-width: 1px;
            border-style: solid;
            border-top-color: black;
            border-left-color: black;
            border-right-color: black;
            border-bottom-color: black;
        }
        .email-layout{
            padding-top: 20px;
            padding-left: 50px;
        }
        .email-input-align{
            margin-top: 10px
        }
        .attach-icon-align{
            margin-top: 10px;
            margin-left: 5px
        }
        .attach-icon-align-send{
            margin-top: 10px;
            margin-left: 25px
        }
        .attach-icon{
            cursor: pointer;
            width: 15px;
            height: 15px;
        }
        .chat-box{
            padding: 10px;
            height: 200px;
            overflow-y: scroll
        }
        .chat-box1{
            height: 100px
        }
        .single-user-align{
            margin-left: 10px
        }
        .single-user-title{
            margin-top: 5px
        }
        .bot-msg{
            flex-direction: column;
            justify-content: center;
            padding: 8px;
            border-top-left-radius: 6px;
            border-top-right-radius: 6px;
            border-bottom-right-radius: 6px;
            margin-top: 5px;
            /* min-width: 120px; */
            min-height: 30px;
            min-width: 95px;
            max-width: 75%;
            background-color: lightgray
        }
        .bot-msg-text{
            color:black; 
            /* text-align: center; */
            font-size: 10px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
        }
        .user-msg{
            flex-direction: column;
            justify-content: center;
            padding: 8px;
            border-top-left-radius: 6px;
            border-top-right-radius: 6px;
            border-bottom-left-radius: 6px;
            margin-top: 5px;
            /* min-width: 120px; */
            min-height: 30px;
            min-width: 95px;
            max-width: 75%;
            background-color: rgb(243, 215, 122)
        }
        .user-msg-text{
            color:black; 
            /* text-align: center; */
            word-wrap: break-word;
            font-size: 10px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
        }
        .sub-align-user-msg{
            flex-flow: row-reverse
        }
        .msg-time{
            width: 75%;
            flex-direction: row;
            justify-content: flex-end
        }
        /* Wave transition */
        
        @keyframes move_wave {
            0% {
                transform: translateX(0) translateZ(0) scaleY(1)
            }
            50% {
                transform: translateX(-25%) translateZ(0) scaleY(0.55)
            }
            100% {
                transform: translateX(-50%) translateZ(0) scaleY(1)
            }
        }
        .waveWrapper {
            overflow: hidden;
            position: absolute;
            left: 0;
            right: 0;
            bottom: 0;
            top: 0;
            margin: auto;
        }
        .waveWrapperInner-2 {
            padding-top:15px;
            position: absolute;
            border-top-right-radius: 6px;
            border-top-left-radius: 6px;
            width: 100%;
            overflow: hidden;
            height: 100%;
            bottom: 0px;
            /* background-color: #86377b */
            /* background-image: linear-gradient(to top, #86377b 20%, #27273c 80%); */
        }
        .waveWrapperInner {
            padding-top:25px;
            position: absolute;
            border-top-right-radius: 6px;
            border-top-left-radius: 6px;
            width: 100%;
            overflow: hidden;
            height: 100%;
            bottom: 0px;
            /* background-color: #86377b */
            /* background-image: linear-gradient(to top, #86377b 20%, #27273c 80%); */
        }
        .bgTop {
            z-index: 15;
            opacity: 0.5;
        }
        .bgTop1 {
            z-index: 15;
            opacity: 1.5;
        }
        .bgMiddle {
            z-index: 10;
            opacity: 0.75;
        }
        .bgBottom {
            z-index: 5;
        }
        .wave {
            position: absolute;
            left: 0;
            width: 200%;
            height: 100%;
            background-repeat: repeat no-repeat;
            background-position: 0 bottom;
            transform-origin: center bottom;
        }
        .waveTop {
            background-size: 100% 0px;
        }
        .waveAnimation .waveTop {
        animation: move-wave 3s;
        -webkit-animation: move-wave 3s;
        -webkit-animation-delay: 1s;
        animation-delay: 1s;
        }
        .waveMiddle {
            background-size: 132% 60px;
        }
        .waveAnimation .waveMiddle {
            animation: move_wave 10s linear infinite;
        }
        .waveBottom {
            background-size: 200% 65px;
        }
        .waveAnimation .waveBottom {
            animation: move_wave 15s linear infinite;
        }
    }
    /* Extra Small Devices, Phones */
    
    @media only screen and (min-width: 768px) {
        .mobile{
            display: none
        }
        .web{
            display: block
        }
        .cursor{
            cursor: pointer;
        }
        .speech-bubble-ds {
            background: #00aabb;
            width: 35px;
            height: 35px;
            border-radius: 8px;
            padding: 15px;
            position: fixed;
        }

        .speech-bubble-ds p {
            margin-bottom: 10px;
        }
        .speech-bubble-ds p:last-of-type {
            margin-bottom: 0;
        }

        .speech-bubble-ds-arrow {
            border-left: 21px solid transparent;
            /* border-top: 20px solid rgba(0, 0, 0, 0.2); */
            bottom: -25px;
            position: absolute;
            right: 15px;
        }
        .speech-bubble-ds-arrow::before {
            border-left: 15px solid transparent;
            border-top: 13px solid #00aabb;
            bottom: 18px;
            content: "";
            position: absolute;
            right: -15px;
        }
        .speech-bubble-ds-arrow::after {
            
        }
        .img-ico1 {
            width: 20px;
            height: 20px;
            cursor: pointer;
        }
        .btn-pos1 {
            bottom: 40px;
            right: 25px;
            justify-content: center;
            cursor: pointer
            /*background-color: #ddd;*/
        }
        /* Step 1 */
        .pop-layout{
            flex-direction: column;

        }
        .welcome-background{
            border-top-right-radius: 6px;
            border-top-left-radius: 6px;
            /* padding-top: 30px; */
            height: 145px;
            /* background-color: #00aabb */
        }
        .header-font{
            color: white;
            text-align: center;
            font-size: 24px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
        }
        .sub-header-font-align{
            margin-left: 15px;
            margin-right: 15px;            
            margin-top: 10px
        }
        .sub-header-font{
            color: white;
            text-align: center;
            font-size: 10px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
        }
        .second-half{
            padding-top: 30px
        }
        .img-layout{
            flex-direction: row;
            justify-content: center
        }
        .img-size{
            height: 25px;
            width: 25px;
            border-width: 0.5px;
            border-color: lightgrey;
            background-color: blueviolet;
            border-radius: 50%;
        }
        .align-sub-header-font-2{
            margin-top: 8px
        }
        .sub-header-font-2{
            text-align: center;
            color:darkgray; 
            font-size: 10px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif
        }
        .msg-btn-align{
            justify-content: center;
            flex-direction: row;
            margin-top: 20px;
        }
        .input-box-layout{
            flex-direction: row;
            margin-top: 0px;
            border-width: 1px;
            border-style: solid;
            border-top-color: darkgray;
            border-left-color: #FFF;
            border-right-color: #FFF;
            border-bottom-color: #FFF;
        }
        .vx-text-field {
             padding: 10px; 
            /* background-color: green; */
            /*padding-top: 25px;*/
            font-size: 10px;
            font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
            width: 80%;
            border-width: 2px;
            border-style: solid;
            border-top-color: #FFF;
            border-left-color: #FFF;
            border-right-color: #FFF;
            border-bottom-color: #FFF;
        }
        
        /* Wave transition */
        
        @keyframes move_wave {
            0% {
                transform: translateX(0) translateZ(0) scaleY(1)
            }
            50% {
                transform: translateX(-25%) translateZ(0) scaleY(0.55)
            }
            100% {
                transform: translateX(-50%) translateZ(0) scaleY(1)
            }
        }
        .waveWrapper {
            overflow: hidden;
            position: absolute;
            left: 0;
            right: 0;
            bottom: 0;
            top: 0;
            margin: auto;
        }
        .waveWrapperInner-2 {
            padding-top:10px;
            position: absolute;
            border-top-right-radius: 6px;
            border-top-left-radius: 6px;
            width: 100%;
            overflow: hidden;
            height: 100%;
            bottom: 0px;
            /* background-color: #86377b */
            /* background-image: linear-gradient(to top, #86377b 20%, #27273c 80%); */
        }
        .waveWrapperInner {
            padding-top:45px;
            position: absolute;
            border-top-right-radius: 6px;
            border-top-left-radius: 6px;
            width: 100%;
            overflow: hidden;
            height: 100%;
            bottom: 0px;
            /* background-color: #86377b */
            /* background-image: linear-gradient(to top, #86377b 20%, #27273c 80%); */
        }
        .bgTop {
            z-index: 15;
            opacity: 0.5;
        }
        .bgMiddle {
            z-index: 10;
            opacity: 0.75;
        }
        .bgBottom {
            z-index: 5;
        }
        .wave {
            position: absolute;
            left: 0;
            width: 200%;
            height: 100%;
            background-repeat: repeat no-repeat;
            background-position: 0 bottom;
            transform-origin: center bottom;
        }
        .waveTop {
            background-size: 100% 0px;
        }
        .waveAnimation .waveTop {
        animation: move-wave 3s;
        -webkit-animation: move-wave 3s;
        -webkit-animation-delay: 1s;
        animation-delay: 1s;
        }
        .waveMiddle {
            background-size: 132% 90px;
        }
        .waveAnimation .waveMiddle {
            animation: move_wave 10s linear infinite;
        }
        .waveBottom {
            background-size: 200% 95px;
        }
        .waveAnimation .waveBottom {
            animation: move_wave 15s linear infinite;
        }

    }
    /* Medium Devices, Desktops */
    
    @media only screen and (min-width: 992px) {
    
    }
</style>