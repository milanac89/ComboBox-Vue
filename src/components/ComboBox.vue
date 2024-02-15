<template>
  <div class="select-one-modal">
    <div @click="selectActiveToggle()"
         class="cursor-pointer align-items-center form-control d-flex position-relative"
         :class="{'select-height-one-modal': multiVar, 'select-custom-main': selectActive && !multiVar, 'select-start-label': multiList.length > 0}">
      <div v-if="!multiVar">{{ selectOptionActive }}</div>
      <div v-else>
        <div class="d-flex flex-wrap" v-if="multiList.length > 0">
          <div v-for="c in multiList" :key="c" class="multi-select-item d-flex mb-1 mt-1">
            <div>{{ c }}</div>
            <img alt="X" @click="removeItem($event)" class="remove-item-multiselect" src="../assets/img/x.svg">
          </div>
        </div>
        <div class="d-flex" v-else>{{ selectOptionActive }}</div>
      </div>
    </div>
    <div class="list-custom-select" v-show="selectActive">
      <div class="position-relative">
        <span v-if="!multiVar" class="floating-label">{{ selectOptionActive }}</span>
        <input id="field-filter-combo" ref="focusInput" type="text" v-model="searchName">
      </div>
      <div class="select-custom">
        <div
                class="d-flex align-items-center justify-content-between" @click="choiseItem(name, $event)" v-for="name in filterNames" :key="name"
                :class="{'option-checkbox': multiVar}"
        >
          {{ name[filterStr] }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ComboBox',
    props: ['model', 'filterStr', 'filterField', 'selected', 'placeholder', 'multiVar', 'closeOther'],
    data() {
        return {
            selectOptionActive: "Выберите...",
            selectActive: false,
            searchName: '',
            multiList: [],
            values: []
        }
    },
    computed: {
        filterNames() {
            return this.model[this.filterField].filter(name =>
                name[this.filterStr]?.toLowerCase().indexOf(this.searchName.toLowerCase()) !== -1)
        }
    },
    watch: {
        selectActive(val) {
            this.$emit('selectActiveChanged', val)
        },
        closeOther() {
            if (this.closeOther != undefined) {
                this.selectActive = false
            }
        }

    },
    mounted() {
        const vm = this

        document.addEventListener('click', e => {
            if (
                vm.selectActive
                && e.target.id !== 'field-filter-combo'
                && !e.target.classList.contains("option-checkbox")
            ) {
                vm.selectActiveToggle()
            }
        })

        if (this.selected) {
            this.selectOptionActive = this.selected[this.filterStr]
            this.selectActive = false
        }

    },
    methods: {
        removeItem(event) {
            let nameText = event.target.previousElementSibling.textContent
            let nameItem = this.multiList.indexOf(nameText)
            this.multiList.splice(nameItem, 1)
            this.values.splice(this.values.indexOf(event.target.previousElementSibling), 1)
            let arrChecks = document.getElementsByClassName('option-checkbox-check')
            for (let arr of arrChecks) {
                if (arr.textContent == nameText) {
                    arr.classList.remove("option-checkbox-check")
                }
            }
            event.stopPropagation()
        },
        setValue(value) {
            this.selectOptionActive = value
            this.selectActive = false


        },
        reset() {
            this.selectOptionActive = "Выберите..."
            this.selectActive = false
            this.values = []
            this.multiList = []
            let choiseArr = document.getElementsByClassName('option-checkbox-check');
            for(let i=0; i< choiseArr.length; i++){
               choiseArr[i].classList.remove("option-checkbox-check");
            }
        },
        selectActiveToggle() {

            if (!this.selectActive && this.closeOther != undefined) {
                this.$emit('updateOther', !this.closeOther)
            }
            this.$nextTick(() => {
                this.selectActive = !this.selectActive
                if (this.selectActive) {
                    this.$nextTick(() => {
                        this.$refs.focusInput.focus()
                    })
                }
            })

            event.stopPropagation()
        },
        choiseItem(e, event) {
            const name = e[this.filterStr]
            if (this.multiVar) {
                event.target.classList.toggle('option-checkbox-check')
                if (event.target.classList.contains("option-checkbox-check")) {
                    if (this.multiList.length == 0) {
                        this.multiList.push(name)
                        this.values.push(e)
                    } else {
                        if (this.multiList.indexOf(name) == -1) {
                            this.multiList.push(name)
                            this.values.push(e)
                        }
                    }
                } else {
                    let num = this.multiList.indexOf(name)
                    if (num !== -1) {
                        this.multiList.splice(num, 1)
                        this.values.splice(num, 1)
                    }
                }
                this.$refs.focusInput.focus()
            } else {
                this.selectOptionActive = e[this.filterStr]
                this.selectActive = false
                this.$emit('selectedIdGet', e)
            }
        }
    },
}
</script>


<style scoped>

</style>
