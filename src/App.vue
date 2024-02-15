<template>
  <div>
    <div class="block">
      <ComboBox ref="user"
                @selectedIdGet="selectedUserId=$event.id"
                :model="model"
                filterStr="title"
                @updateOther="closeOther = $event"
                :closeOther="closeOther"
                filterField="users">
      </ComboBox>
    </div>
    <div class="block">
      <ComboBox ref="indicators"
                 :model="model"
                 filterStr="title"
                 filterField="indicators"
                 :closeOther="closeOther"
                 @updateOther="closeOther = $event"
                 :multiVar="true">
      </ComboBox>
    </div>
    <button @click="save()">Ок</button>
  </div>
</template>

<script>
import ComboBox from './components/ComboBox.vue'

export default {
  name: 'App',
  components: {
      ComboBox
  },
    data() {
        return {
            selectedUserId: null,
            model: {
                users: [
                    {
                      id: 1,
                      title: "Ivan Ivanov"
                    },
                    {
                        id: 2,
                        title: "Petr Petrov"
                    },
                    {
                        id: 3,
                        title: "Lina Linova"
                    },
                ],
                indicators: [
                    {
                        title: "Сахар",
                        group: "one"
                    },
                    {
                        group: "two",
                        title: "Холестерин"
                    },
                    {
                        group: "one",
                        title: "Пульс"
                    },
                ]
            },
            closeOther: true
        }
    },
    methods: {
        getIndicators() {
            return this.$refs.indicators.values
        },
        reset() {
            this.selectedUserId = null
            this.$refs.indicators.reset()
            this.$refs.user.reset()
        },
        save(){
            const indicators = this.getIndicators()
            console.log(this.selectedUserId, indicators)
            this.reset();
        }
    }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
