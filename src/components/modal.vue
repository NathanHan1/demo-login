<template>
    <Modal
        v-model="currentmode"
        title="新建项目"
        class="modal"
        @on-ok="ok"
        @on-cancel="cancel"
        >
        <p>啦啦啦啦</p>
        <p>啦啦啦啦</p>
        <p>啦啦啦啦</p>
        <p>啦啦啦啦</p>
    </Modal>
</template>

<script>
export default {
    props: {
        mode: {
            type: Boolean
        }
    },
    computed: {
        currentmode: {
            get:function () {
                return this.mode
            },
            set: function () {
            }
        }
    },
    methods: {
        cancel() {
            this.resetMode()
        },
        ok() {
            this.resetMode()
        },
        resetMode() {
            this.$emit('reset')
        }
    }
}
</script>
