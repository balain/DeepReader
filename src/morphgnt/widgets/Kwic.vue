<template>
  <widget>
    <span slot="header">KWIC</span>
    <span slot="summary" v-if="word">
      {{ word.word }}
    </span>
    <div slot="body" v-if="results">
      <table>
        <tr v-for="result in results">
          <td class="pre">{{ result.pre }}</td>
          <td class="keyword">{{ result.keyword }}</td>
          <td class="post">{{ result.post }}</td>
        </tr>
      </table>
    </div>
  </widget>
</template>

<script>
import Widget from '@/components/Widget';
import JsonObject from '@/components/JsonObject';
import morphgnt from '@/morphgnt';

export default {
  computed: {
    word() {
      return this.$store.state.selectedWord;
    },
  },
  data() {
    return {
      results: null,
    };
  },
  watch: {
    word: 'fetchData',
  },
  methods: {
    fetchData() {
      morphgnt.kwic(this.word.word).then((results) => {
        this.results = results;
      });
    },
  },
  components: {
    Widget,
    JsonObject,
  },
};
</script>

<style lang="scss">
  table {
    width: 100%;
    .pre {
      text-align: right;
      white-space: nowrap;
    }
    .keyword {
      white-space: nowrap;
      font-weight: bold;
      text-align: center;
    }
    .post {
      text-align: left;
      white-space: nowrap;
    }
  }
</style>
