<template>
    <div class="ivu-select-dropdown" v-if="show" :class="className" :style="styles"><slot></slot></div>
</template>
<script>
    import Vue from 'vue';
    const isServer = Vue.prototype.$isServer;
    import { getStyle } from '../../utils/assist';
    const Popper = isServer ? function() {} : require('popper.js/dist/umd/popper.js');  // eslint-disable-line

    import { transferIndex, transferIncrease } from '../../utils/transfer-queue';

    export default {
        name: 'Drop',
        props: {
            placement: {
                type: String,
                default: 'bottom-start'
            },
            className: {
                type: String
            },
            transfer: {
                type: Boolean
            },
            dropVisible: {
                type: [Boolean , Number]
            }
        },
        data () {
            return {
                popper: null,
                width: '',
                show : false,
                timeoutId : -1,
                popperStatus: false,
                tIndex: this.handleGetIndex()
            };
        },
        watch : {
            dropVisible(){
                if(this.dropVisible == -1){
                    this.update();
                    this.show = true;
                    return;
                }
                if(this.dropVisible){
                    clearTimeout(this.timeoutId)
                    this.update()
                    this.show = true;
                }else{
                    this.destroy();
                    // this.timeoutId = setTimeout(() => {
                    // this.destroy();
                    //     this.show = false;
                    // } , 600)
                }
            }
        },
        computed: {
            styles () {
                let style = {};
                if (this.width) style.minWidth = `${this.width}px`;

                if (this.transfer) style['z-index'] = 1060 + this.tIndex;

                return style;
            }
        },

        methods: {
            update () {
                if(this.dropVisible == false) return;

                if (isServer) return;
                if (this.popper) {
                    this.$nextTick(() => {
                        this.popper.update();
                        this.popperStatus = true;
                    });
                } else {
                    this.$nextTick(() => {
                        this.popper = new Popper(this.$parent.$refs.reference, this.$el, {
                            placement: this.placement,
                            modifiers: {
                                computeStyle:{
                                    gpuAcceleration: false
                                },
                                preventOverflow :{
                                    boundariesElement: 'window'
                                }
                            },
                            onCreate:()=>{
                                this.resetTransformOrigin();
                                this.$nextTick(this.popper.update());
                            },
                            onUpdate:()=>{
                                this.resetTransformOrigin();
                            }
                        });
                    });
                }
                // set a height for parent is Modal and Select's width is 100%
                if (this.$parent.$options.name === 'iSelect') {
                    this.width = parseInt(getStyle(this.$parent.$el, 'width'));
                }
                this.tIndex = this.handleGetIndex();
            },
            destroy () {
                if (this.popper) {
                    setTimeout(() => {
                        if (this.popper && !this.popperStatus) {
                            this.popper.destroy();
                            this.popper = null;
                        }
                        this.popperStatus = false;
                    }, 300);
                }
            },
            resetTransformOrigin() {
                // 不判断，Select 会报错，不知道为什么
                if (!this.popper) return;

                let x_placement = this.popper.popper.getAttribute('x-placement');
                let placementStart = x_placement.split('-')[0];
                let placementEnd = x_placement.split('-')[1];
                const leftOrRight = x_placement === 'left' || x_placement === 'right';
                if(!leftOrRight){
                    this.popper.popper.style.transformOrigin = placementStart==='bottom' || ( placementStart !== 'top' && placementEnd === 'start') ? 'center top' : 'center bottom';
                }
            },
            handleGetIndex () {
                transferIncrease();
                return transferIndex;
            },
        },
        created () {
            this.$on('on-update-popper', this.update);
            this.$on('on-destroy-popper', this.destroy);
            if(this.dropVisible == -1){
                this.update();
                this.show = true;
                return;
            }
        },
        beforeDestroy () {
            if (this.popper) {
                this.popper.destroy();
            }
        }
    };
</script>