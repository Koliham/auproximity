<template>
  <v-dialog v-model="dialog" :max-width="options.width" :style="{ zIndex: options.zIndex }" @keydown.esc="cancel">
    <v-card>
      <v-toolbar dark :color="options.color" dense flat>
        <v-toolbar-title class="white--text">{{ title }}</v-toolbar-title>
      </v-toolbar>
      <v-card-text v-show="!!message" class="pa-4">{{ message }} sda</v-card-text>
      <v-form v-model="valid">
        <v-text-field
          v-model="gameCode"
          label="Game Code"
          counter="6"
          maxlength="6"
          outlined
        ></v-text-field>
        <input :value="shareSlug" id="slug-share">
      </v-form>
      <v-card-actions class="pt-0">
        <v-spacer></v-spacer>
        <v-btn color="primary darken-1" text @click.native="agree">Update</v-btn>
        <v-btn color="grey" text @click.native="cancel">Cancel</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
<script lang="ts">

import {
    BackendType,
    PublicLobbyRegion
} from '@/models/BackendModel'
import { Vue } from 'vue-property-decorator'

export default class ConfirmLobbyDetails extends Vue {
    valid = true;
    dialog = false;
    resolve = (value: any): void => {};
    reject = (value: any): void => {};
    message: string = "";
    title: string = "";
    options = {
        color: 'primary',
        width: 290,
        zIndex: 200
    };

    open (title: string, message: string, options: {}) {
        this.dialog = true
        this.title = title
        this.message = message
        this.options = Object.assign(this.options, options)
        return new Promise<any>((resolve, reject) => {
            this.resolve = resolve
            this.reject = reject
        });
    }

    agree () {
        this.dialog = false
        this?.resolve(true)
    }

    cancel () {
        this.dialog = false
        this?.resolve(false)
    }
}
</script>
