<template>
  <v-card class="pa-5">
    <h3>Host options</h3>
    <v-form>
      <v-slider
        label="Falloff"
        min="0.5"
        max="10"
        step="0.1"
        v-model="$store.state.options.falloff"
        :readonly="!$store.state.ishost"
        :disabled="$store.state.options.falloffVision || !$store.state.joinedRoom"
        @change="updateOptions"
      >
        <template v-slot:append>
          <v-text-field
            v-model="$store.state.options.falloff"
            class="mt-0 pt-0"
            type="number"
            style="width: 60px"
          ></v-text-field>
        </template>
      </v-slider>
      <v-checkbox
        label="Vision falloff"
        v-model="$store.state.options.falloffVision"
        :readonly="!$store.state.ishost"
        :disabled="!$store.state.joinedRoom"
        @change="updateOptions"
      ></v-checkbox>
      <v-checkbox
        label="Wall collision"
        v-model="$store.state.options.colliders"
        :readonly="!$store.state.ishost"
        :disabled="!$store.state.joinedRoom"
        @change="updateOptions"
      ></v-checkbox>
      <v-checkbox
        label="PA System"
        v-model="$store.state.options.paSystems"
        :readonly="!$store.state.ishost"
        :disabled="!$store.state.joinedRoom"
        @change="updateOptions"
      ></v-checkbox>
      <v-btn
          color="success"
          class="mr-4"
          type="button"
          @click="updateLobbyDeails"
        >
          Lobby Details
        </v-btn>
    </v-form>
    <ConfirmLobbyDetails ref="updateLobbyDetails"></ConfirmLobbyDetails>
  </v-card>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import { Socket } from 'vue-socket.io-extended'
import ClientSocketEvents from '@/models/ClientSocketEvents'
import { HostOptions as HostOptionsModel } from '@/models/RoomModel'
import ConfirmLobbyDetails from '@/components/ConfirmLobbyDetails.vue'

@Component({
  components: {
    ConfirmLobbyDetails
  }
})
export default class HostOptions extends Vue {

  popupRef: any = this.$refs.updateLobbyDetails;

  updateLobbyDeails () {
    console.log(this.$refs.updateLobbyDetails)
    this.popupRef.open('Test', 'Are you sure?', { color: 'red' }).then((confirm: boolean) => { console.log(confirm) })
  }

  updateOptions () {
    this.$socket.client.emit(
      ClientSocketEvents.SetOptions,
      { options: this.$store.state.options }
    )
  }

  @Socket(ClientSocketEvents.SetOptions)
  onSetPose (payload: { options: HostOptionsModel }) {
    this.$store.state.options = payload.options
  }
}
</script>
<style scoped lang="stylus"></style>
