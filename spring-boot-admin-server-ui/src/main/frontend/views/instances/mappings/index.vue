<!--
  - Copyright 2014-2018 the original author or authors.
  -
  - Licensed under the Apache License, Version 2.0 (the "License");
  - you may not use this file except in compliance with the License.
  - You may obtain a copy of the License at
  -
  -     http://www.apache.org/licenses/LICENSE-2.0
  -
  - Unless required by applicable law or agreed to in writing, software
  - distributed under the License is distributed on an "AS IS" BASIS,
  - WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  - See the License for the specific language governing permissions and
  - limitations under the License.
  -->

<template>
  <section class="section" :class="{ 'is-loading' : !hasLoaded }">
    <div class="container">
      <div v-if="error" class="message is-danger">
        <div class="message-body">
          <strong>
            <font-awesome-icon class="has-text-danger" icon="exclamation-triangle"></font-awesome-icon>
            Fetching mappings failed.
          </strong>
        </div>
      </div>
      <div class="content" v-if="mappings">
        <div class="field has-addons">
          <p class="control is-expanded">
            <input class="input" type="text" placeholder="name / value" v-model="filter">
          </p>
        </div>
      </div>
      <div class="content" v-if="mappings">
        <sba-panel>
          <table class="table">
            <tr v-for="(value, key) in mappings" :key="key">
              <td class="info__key" v-text="key"></td>
              <td>
                <sba-formatted-obj :value="value"></sba-formatted-obj>
              </td>
            </tr>
          </table>
          <p v-else class="is-muted">No mappings found.</p>
        </sba-panel>
      </div>
    </div>
  </section>
</template>

<script>
  export default {
    props: ['instance'],

    data: () => ({
      hasLoaded: false,
      error: null,
      mappings: null,
      filter: null
    }),
    created() {
      this.fetchMappings();
    },
    watch: {
      instance() {
        this.fetchMappings();
      }
    },
    methods: {
      async fetchMappings() {
        if (this.instance) {
          this.error = null;
          try {
            const res = await this.instance.fetchMappings();
            this.env = res.data;
          } catch (error) {
            console.warn('Fetching mappings failed:', error);
            this.error = error;
          }
          this.hasLoaded = true;
        }
      }
    }
  }
</script>
