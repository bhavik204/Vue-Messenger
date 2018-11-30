<template>
    <div>
        <!-- Icon button -->
        <div v-if="withIcon === true" class="vx-primary-btn" :style="{ backgroundColor:btnColor}">
            <div v-if="leftActive === true" class="vx-primary-btn-margin-right">
                <image :src="btnIcon" class="vx-primary-btn-icon-size" />
            </div>
            <text class="vx-primary-btn-label" :style="{fontSize: labelSize}">{{ btnName }}</text>
            <div v-if="rightActive === true" class="vx-primary-btn-margin-left">
                <image :src="btnIcon" class="vx-primary-btn-icon-size" />
            </div>
        </div>

        <!-- Without icon button -->
        <div v-if="withoutIcon === true" class="vx-primary-btn" :style="{backgroundColor:btnColor}">
            <text class="vx-primary-btn-label" :style="{fontSize: labelSize}">{{ btnName }}</text>
        </div>

    </div>
    
</template>
<script>
    export default {
        props: {
            btnName: {
                type: String,
                default: 'Demo'
            },
            btnIcon: String,
            btnWidth: String,
            btnHeight: String,
            iconSide: {
                type:String,
                default: 'right'
            },
            btnColor: {
                type: String,
                default: '#3b371e'
            },
            iconButton: {
                type: Boolean,
                default: true
            },
            labelSize: String
        },
        data() {
            return {
                leftActive: true,
                rightActive: false,
                withIcon: true,
                withoutIcon: false
            }
        },
        created() {
            console.log('color',this.btnColor)
            if(this.iconSide === 'right') {
                this.rightActive = true;
                this.leftActive = false;
            } 
            
            // ICON button

            if(this.iconButton === true) {
                this.withIcon = true;
                this.withoutIcon = false;
            } else if(this.iconButton === false) {      /** Without icon */
                this.withIcon = false;
                this.withoutIcon = true;
            }
        }
    }
</script>
<style>

    .vx-primary-btn {
        background-color: #3b371e;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        border-radius: 4px;
        height: 60px;
    }
    .vx-primary-btn-label{
        font-size : 24px;
        font-weight: 500;
        color:#FFFFFF;
        font-family:robotoregular;
    }
    .vx-primary-btn-margin-right {
        margin-right: 20px;
    }
    .vx-primary-btn-margin-left {
        margin-left: 20px;
    }
    .vx-primary-btn-icon-size {
        width: 30px;
        height: 30px;
    }
    @media (min-width: 320px) {
        .vx-primary-btn {
            border-radius: 6px;
            height: 25px;
            width: 140px;
            position: relative;
            cursor: pointer;
        }
        .vx-primary-btn-label{
            font-size : 10px;
            font-weight: 500;
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
    }
    @media (min-width: 768px) {

    }
    @media (min-width: 992px) {
        
    }


</style>

