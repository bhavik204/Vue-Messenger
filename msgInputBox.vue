<template>
    <!--Animation -->
    <div style="background-color: white;" :style="{'background-color': bgColor}">
        <div class="vx-input-box" >
        <input  class="vx-text-field" 
            :type="vxType"
            :value="vxModel" 
            :disabled="fieldDisabled" 
            :placeholder="vxPlaceholder"
            :style="{borderBottomColor: erroStyle.btmBorder}"
            :min="minDate"
            @input="onInput" 
            @focus="applyAnimation"
            @blur="removeAnimation"  
            @change="processEvent">

        <text ref="test" 
            class="vx-label-font vx-lable-position" 
            :class="['vx-fixed-label', 'fixed_label']" 
            :style="{color: labelColor}" >{{ vxLabel }}</text>
        </div>
        <!-- <div v-if="childinp == true">
            <text class="vx-err-font" v-if="hasRequired === true">{{message}}</text>
        </div> -->

        <div v-if="showErrorText === true">
            <text class="vx-err-font">{{errorText}}</text>
        </div>
    </div>
  </template>


<script>
    const animation = weex.requireModule('animation');
    import moment from 'moment'
    // const errorList = [
    //     "Invalid, please enter alphabets only",
    //     "Not eligilbe, age should be greater than 18",
    // ]
    export default {
        props: {
            vxType: {
                type: String,
                default: 'text'
            },
            vxModel: {
                type: [String, Number],
                default: ''
            },
            message: {
                type: String,
                default: 'This field is required'
            },
            vxLabel: String,
            vxPlaceholder: String,
            bgColor: String,
            fieldDisabled: {
                type: Boolean,
                default: false
            },
            minDate:{
                type:String,
                default:'momentDte'
                
            },
            hasRequired: {
                type: Boolean,
                default: false
            }
        },
        created(){
            console.log('3712893296392642489')
        },
        data: function() {
            return {
                moment:moment,
                momentDte:'',
                childinp: false,
                labelColor: '#999999',
                fixed_label: false,
                value: '',

                erroStyle: {
                    btmBorder: '#999999'
                },

                showErrorText: false, /** This is used to make visible error text when  hasRequired === true*/
                setBit: 0,
                findDigit: 'No',
                errorText: ''
            }
        },
        created() {
            console.log('3712893296392642489')
            var dat = new Date()
            this.momentDte = moment(dat).format('YYYY-MM-DD')
            if (this.vxPlaceholder !== '') {
                this.fixed_label = true
            }
            // console.log(this.vxType.type ,this.vxType , 'VX type')

            if (this.vxType == "date" || this.vxType == "tel" || this.vxType == "time" || this.vxType == "file") {
                // console.log( this.vxType , 'VX type inside')
                this.fixed_label = true
            }
            // this.value = this.vxModel;

        },
        methods: {

            applyAnimation() {

                //. To apply black color @text lbl on focus @web and APK / IOS
                if (this.value == "" && this.hasRequired === true && this.setBit === 1 ) {

                    this.labelColor = '#fa0909';    /** Red Color */
                    this.errorText = this.message
                } else {
                    if (this.findDigit === 'Yes') {
                        this.labelColor = '#fa0909';    /** Red Color */
                        this.findDigit = 'No'
                    } else {
                        this.labelColor = '#3a3a3a';   /** Gray or black color */
                    }
                }
                // this.vxModel = this.value;
                if (this.fixed_label !== true) {
                    var testEl = this.$refs.test;
                    animation.transition(testEl, {
                        styles: {
                            // color: '#FF0000',
                            transform: 'translate(0px, -150%) scale(1)',
                            transformOrigin: 'center center'
                        },
                        duration: 150, //ms
                        timingFunction: 'ease-out',
                        delay: 0 //ms
                    }, function() {

                    })
                    this.labelColor = '#3a3a3a';
                    // this.vxModel = this.value;
                }

            },

            /** This function is called on blur event is fired*/

            removeAnimation(getValue) {
                //   this.value = getValue.value;
                //   console.log(this.value)
                //             this.$emit('valueReading', {value: this.value});
                //. To Remove Active  color @text lbl on focus @web and APK / IOS
                this.labelColor = '#999';
                //   this.vxModel = this.value;
                if (this.value == '') {
                    console.log(this.value + 'No Data');
                    var testEl = this.$refs.test;
                    animation.transition(testEl, {
                        styles: {
                            // color: '#FF0000',
                            transform: 'translate(0, 0)',
                            transformOrigin: 'center center'
                        },
                        duration: 150, //ms
                        timingFunction: 'ease-out',
                        delay: 0 //ms
                    }, function() {

                    })
                    this.labelColor = '#999999'
                    this.setBit = 1
                    // this.$emit('valueReading', {value: this.value});
                }
                //}
                this.errorMsgActive(this.value)
                if (this.vxType === 'text') {
                    this.checkDigitis(this.value)
                }
            },
            processEvent(getValue) {
                this.value = getValue.value;
                this.$emit('onChange', {
                    value: this.value
                });
            },
            onInput(getValue) {
                this.value = getValue.value;

                this.$emit('valueReading', {
                    value: this.value
                });
                this.errorMsgActive(this.value)
            },

            /** This function checks value is avalible or not
             * also make this field is mandotory 
             */
            errorMsgActive(val) {
                
                if (val == "" && this.hasRequired === true) {
                    this.showErrorText = true;
                    this.erroStyle.btmBorder = '#fa0909';
                    this.labelColor = '#fa0909';
                    this.errorText = this.message
    
                } else {
                    this.showErrorText = false;
                    this.labelColor = '#3a3a3a';
                    this.erroStyle.btmBorder = '#999999';
                }
            },

            /**
             *  this function is used to identify digitis in vx-input-box
             */

            checkDigitis(_input) {
                _input = _input.split('');
                let chD;
                for(let i = 0; i < _input.length; i++) {
                    chD = parseInt(_input[i]);
                    if(isNaN(chD)) {
                        // Pass
                    } else {
                        this.showErrorText = true;
                        this.erroStyle.btmBorder = '#fa0909';
                        this.labelColor = '#fa0909';
                        this.errorText = 'Invalid, Please enter alphabets only'  
                        this.findDigit = 'Yes' 
                    }
                } 
            },

            // DOB(dob) {
                
            // }
        }
    }
</script>

<style scoped>
    .robotoregular {
        font-family: robotoregular;
    }
    
    .vx-input-box {
        padding: 20px 0;
        padding-top: 30px;
    }
    
    .vx-label-font {
        font-size: 24px;
        color: #999999;
    }
    
    .vx-lable-position {
        position: absolute;
        top: 55px;
        left: 5px;
    }
    
    .vx-fixed-label {
        position: absolute;
        top: 0px;
        left: 5px;
    }
    
    .vx-text-field {
        padding: 20px;
        padding-left: 4px;
        /* background-color: green; */
        /*padding-top: 25px;*/
        font-size: 28px;
        width: 750px;
        border-width: 2px;
        border-style: solid;
        border-top-color: #FFF;
        border-left-color: #FFF;
        border-right-color: #FFF;
        border-bottom-color: #999999;
    }
    
    .vx-text-field:focus {
        border-width: 2px;
        border-bottom-color: #3a3a3a;
    }
    
    .vx-err-font {
        font-size: 20px;
        /* padding-top: 10px; */
        color: #9d9d9d;
        font-family: robotoregular;
    }
    
    @media (min-width: 320px) {
        .vx-err-font {
            font-size: 10px;
            padding-top: 0px;
            color: #fa0909;
            font-family: robotoregular;
        }
         ::-webkit-input-placeholder {
            color: #ddd;
        }
        .vx-text-field {
            padding: 10px 0PX;
            font-size: 14px;
            width: 100%;
            padding-bottom: 10px;
            border-width: 1px;
        }
        .vx-text-field:focus {
            border-width: 1px;
        }
        .vx-label-font {
            font-size: 14px;
        }
        .vx-lable-position {
            top: 21px;
            left: 0px;
        }
        .vx-fixed-label {
            position: absolute;
            top: 0;
            left: 0px;
        }
        .vx-input-box {
            padding: 5px 0px;
            padding-top: 15px;
        }
    }
    
    @media screen and (min-width: 768px) {}
    
    @media screen and (min-width: 992px) {}
</style>