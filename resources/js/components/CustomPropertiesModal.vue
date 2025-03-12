<template>
    <Modal
        :show="true"
        maxWidth="2xl"
        @modal-close="handleClose"
        :classWhitelist="[
            'flatpickr-current-month',
            'flatpickr-next-month',
            'flatpickr-prev-month',
            'flatpickr-weekday',
            'flatpickr-weekdays',
            'flatpickr-calendar',
        ]"
        @click.self="handleOutsideClick"
     >
        <card class="overflow-hidden">
            <form class="rounded-lg shadow-lg overflow-hidden w-action-fields"
                @submit.prevent="handleUpdate"
                autocomplete="off"
            >
                <div class="bg-gray-100 dark:bg-gray-700 px-6 py-3 flex border-b dark:border-gray-700">
                    <div class="flex items-center ml-auto">
                        <button type="button" class="btn text-sm font-bold h-9 px-3 mr-3 btn-link rounded focus:ring ring-primary-200 dark:ring-gray-600 text-gray-400 hover:text-gray-300 active:text-gray-500 dark:text-gray-500 dark:hover:text-gray-400 dark:active:text-gray-600 dark:hover:bg-gray-800" @click.prevent="handleClose">
                            {{__('Cancel')}}
                        </button>

                        <button type="submit" class="btn btn-default btn-primary shadow bg-primary-500 hover:bg-primary-400 text-white dark:text-gray-900 rounded bg-primary-500 hover:bg-primary-400 text-white dark:text-gray-900 p-2 px-4">
                            {{__('Update')}}
                        </button>
                    </div>
                </div>
                <div v-for="(field, index) in fields" :key="field.attribute" class="action">
                    <component
                        :key="index"
                        :index="index"
                        :is="`form-${field.component}`"
                        :field="field"
                        :show-help-text="true"
                    />
                </div>
            </form>
        </card>
    </Modal>
</template>

<script>
  export default {
    props: {
        fields: {
            type: Array,
            required: true,
        }
    },

    methods: {
        handleClose () {
            this.$emit('close')
        },

        handleUpdate () {
            let formData = new FormData()

            this.fields.forEach(field => field.fill(formData))

            this.$emit('update', formData)
        },

        handleOutsideClick () {
            this.handleClose()
        }
    }
  }
</script>
