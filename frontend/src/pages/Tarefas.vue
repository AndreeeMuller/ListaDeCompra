<template>
  <q-page class="column" :class="$q.dark.isActive ? 'bg-grey-8': 'bg-grey-3'">
    <div class="row q-pa-sm bg-primary">
      <q-input  class="col"
                bg-color="white"
                placeholder="Adicionar Tarefa"
                v-model="novaTarefa"
                @keyup.enter="adicionarTarefa"
                square
                filled
                dense>
          <template v-slot:append>
            <q-btn  @click="adicionarTarefa"
                    dense
                    flat
                    icon="add">
            </q-btn>
          </template>
      </q-input>
    </div>
    <q-list class="bg-white"
            separator
            bordered>
      <q-item   v-ripple
                clickable
                v-for="(tarefa, index) in tarefas"
                :key="index"
                @click="tarefa.check = !tarefa.check; salvarTarefa()"
                :class="{ 'check bg-yellow-1': tarefa.check }">
        <q-item-section avatar>
          <q-checkbox   v-model="tarefa.check"
                        class="no-pointer-events"
                        color="primary">
          </q-checkbox>
        </q-item-section>
        <q-item-section>
          <q-item-label>
            {{ tarefa.titulo }}
          </q-item-label>
        </q-item-section>
        <q-item-section v-if="tarefa.check"
                        side>
          <q-btn  flat
                  round
                  dense
                  color="primary"
                  icon="delete"
                  @click.stop="excluirTarefa(index)">
          </q-btn>
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-tasks absolute-center text-center" v-if="!tarefas.length">
      <q-icon name="check"
              size="100px"
              :color="$q.dark.isActive ? 'secondary' : 'primary'">
      </q-icon>
      <div class="text-h5 text-center" :class="$q.dark.isActive ? 'text-secondary' : 'text-primary'">
        Sem tarefas
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data () {
    return {
      novaTarefa: '',
      tarefas: this.$q.localStorage.getItem('tarefas') || []
    }
  },
  methods: {
    excluirTarefa (index) {
      this.$q.dialog({
        title: 'Confirme',
        message: 'Tem certeza que deseja excluir?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tarefas.splice(index, 1)
        this.$q.notify({
          type: 'positive',
          message:'Tarefa excluída'
        })
      this.salvarTarefa()
      })
    },
    adicionarTarefa () {
      this.tarefas.push({
        titulo: this.novaTarefa,
        check: false
      })
      this.salvarTarefa()
      this.novaTarefa = ''
    },
    salvarTarefa () {
      this.$q.localStorage.set('tarefas', this.tarefas)
    }
  }
}
</script>
