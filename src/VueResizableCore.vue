<template>
    <vue-draggable-core
		:class="classes"
		:style="styles"
		:onStart="resizeHandler('onResizeStart')"
		:onDrag="resizeHandler('onResize')"
		:onStop="resizeHandler('onResizeStop')"
		:noTouchAction="noTouchAction"
		:touchAction="touchAction"
		:noSelection="noSelection"
		:cancel="cancel"
		:disabled="disabled"
		:grid="grid"
    >
        <slot></slot>
    </vue-draggable-core>
</template>

<script>
	import VueDraggableCore from 'vue-draggable-core';
export default {
	
	components: {
		VueDraggableCore
	},
    data() {
        return {
            resizing: false,
            slackW: 0,
            slackH: 0,
            width: 0,
            height: 0
        }
    },
    props: {
        noSelection : {
            type: Boolean,
            default: true,
        },
        noTouchAction : {
            type: Boolean,
            default: true,
        },
        touchAction: {
            type: String,
            default: 'none',
        },
        cancel: {
            type: String,
            default: null
        },
        disabled: {
            type: Boolean,
            default: false
        },
        className: {
            type: String,
            required: false,
            default: ""
        },
        w: {
            type: Number,
            required: true
        },
        h: {
            type: Number,
            required: true
        },
        grid: {
            type: Array,
            required: false,
            default: () => [1, 1]
        },
        lockAspectRatio: {
            type: Boolean,
            required: false,
            default: false
        },
        //'both' | 'x' | 'y' | 'none';
        axis: {
            type: String,
            required: false,
            default: 'both'
        },
        minConstraints: {
            type: Array,
            required: false,
            default: () => [20, 20]
        },
        maxConstraints: {
            type: Array,
            required: false,
            default: () => [Infinity, Infinity]
        },
        onResizeStart: {
            type: Function,
            required: false,
            default: () => {}
        },
        onResizeStop: {
            type: Function,
            required: false,
            default: () => {}
        },
        onResize: {
            type: Function,
            required: false,
            default: () => {}
        },
    },
	watch: {
	    w(val) {
	        if (val)
	        	this.width = this.w;
		},
		h(val) {
            if (val)
	        	this.height = this.h;
		}
	},
    mounted() {
        this.width = this.w;
        this.height = this.h;
    },
    computed: {
		classes() {
			if (this.className)
				return { [this.className]: true };
			else
				return {};
		},
        styles() {
            return {
                cursor: 'se-resize',
                'touch-action': (this.noTouchAction) ? false : this.touchAction,
            }
        }
    },
    methods: {
        lockAspectRatioFunc(width, height, aspectRatio) {
            height = width / aspectRatio;
            width = height * aspectRatio;

            return [width, height];
        },
        // If you do this, be careful of constraints
        runConstraints(width, height) {
            const [min, max] = [this.minConstraints, this.maxConstraints];

            if (this.lockAspectRatio) {
                const ratio = this.width / this.height;
                height = width / ratio;
                width = height * ratio;
            }

            if (!min && !max) return [width, height];

            const [oldW, oldH] = [width, height];

            return [width, height];
        },
        resizeHandler(handlerName) {
            return (e, {node, deltaX, deltaY}) => {

                // Axis restrictions
                const canDragX = this.axis === 'both' || this.axis === 'x';
                const canDragY = this.axis === 'both' || this.axis === 'y';


                if ( this.width === 0)
                    this.width = this.w;
                if ( this.height === 0)
                    this.height = this.h;

                // Update w/h
                let width = this.width + (canDragX ? deltaX : 0);
                let height = this.height + (canDragY ? deltaY : 0);


                // Early return if no change
                const widthChanged = width !== this.width, heightChanged = height !== this.height;
                if (handlerName === 'onResize' && !widthChanged && !heightChanged) return;

                [width, height] = this.runConstraints(width, height);

                // Set the appropriate state for this handler.
                const newState = {};
                if (handlerName === 'onResizeStart') {
                    newState.resizing = true;
                } else if (handlerName === 'onResizeStop') {
                    newState.resizing = false;
                    newState.slackW = 0;
					newState.slackH = 0;
                } else {
                    // Early return if no change after constraints
                    if (width === this.width && height === this.height) return;
                    newState.width = width;
                    newState.height = height;
                }


                const hasCb = typeof this[handlerName] === 'function';

                if (hasCb) {
                    this.resizing = newState.resizing;
                    this.slackW = newState.slackW;
                    this.width = newState.width ? newState.width : this.width;
                    this.height = newState.height ? newState.height : this.height;

                    this[handlerName](e, {node, size: {width, height}});

                } else {

                    this.resizing = newState.resizing;
                    this.slackW = newState.slackW;
                    this.width = newState.width ? newState.width : this.width;
                    this.height = newState.height ? newState.height : this.height;
					
					this.$emit(handlerName,  e, {node, size: {width, height} });
                }
            };
        },
    },
}

</script>