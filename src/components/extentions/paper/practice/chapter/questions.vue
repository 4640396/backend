<style lang="less" scoped>
.paper-quesiton {
  width: 100%;
  height: 500px;
  float: left;
  overflow-y: auto;
  border: 1px solid #aaa;
  padding: 15px;

  .paper-item {
    width: 100%;
    height: auto;
    float: left;
    background-color: rgba(0, 0, 0, 0.02);
    padding: 15px;
    border-radius: 15px;
    margin-top: 10px;

    &:hover {
      cursor: pointer;
      background-color: rgba(0, 0, 0, 0.04);
    }

    .info {
      color: rgba(0, 0, 0, 0.4);
      font-size: 0.8rem;
      margin-bottom: 5px;
    }
    .content {
      color: #333;
    }
  }
}

.question-box {
  position: relative;
  width: 100%;
  height: 500px;
  float: left;

  .filter-box {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    width: 100%;
    height: 60px;
    padding: 10px 15px;
    background-color: rgba(0, 0, 0, 0.02);
  }
  .body {
    margin-top: 60px;
    width: 100%;
    height: 440px;
    float: left;
    overflow-y: auto;
    background-color: rgba(0, 0, 0, 0.03);
    padding: 15px;

    .paper-item {
      width: 100%;
      height: auto;
      float: left;
      background-color: rgba(0, 0, 0, 0.02);
      padding: 15px;
      border-radius: 15px;
      margin-top: 10px;

      &:hover {
        cursor: pointer;
        background-color: rgba(0, 0, 0, 0.04);
      }

      .info {
        color: rgba(0, 0, 0, 0.4);
        font-size: 0.8rem;
        margin-bottom: 5px;
      }
      .content {
        color: #333;
      }
    }
  }
}
</style>
<template>
  <div class="h-panel w-1000">
    <div class="h-panel-bar">
      <span class="h-panel-title">指定试题</span>
    </div>
    <div class="h-panel-body">
      <div class="float-box mb-10">
        <Row :space="30">
          <Cell :width="12">
            <div class="paper-quesiton">
              <div class="title">已选择{{datas.length}}道试题(点击试题可删除)</div>
              <div
                class="paper-item"
                @click="deleteQuestion(question)"
                v-for="question in datas"
                :key="question.id"
              >
                <div
                  class="info"
                >ID:{{question.id}}|{{question.type_text}}|{{question.level_text}}|{{question.score}}分</div>
                <div class="content" v-html="question.content"></div>
              </div>
            </div>
          </Cell>
          <Cell :width="12">
            <div class="question-box">
              <div class="filter-box">
                <Select
                  v-model="filter.category_id"
                  :datas="categories"
                  keyName="id"
                  titleName="name"
                  :filterable="true"
                  @change="categoryChange"
                ></Select>
              </div>
              <div class="body">
                <div
                  class="paper-item"
                  @click="addQuestion(question)"
                  v-for="question in questions"
                  :key="question.id"
                >
                  <div
                    class="info"
                  >ID:{{question.id}}|{{question.type_text}}|{{question.level_text}}|{{question.score}}分</div>
                  <div class="content" v-html="question.content"></div>
                </div>
              </div>
            </div>
          </Cell>
        </Row>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ['id'],
  data() {
    return {
      filter: {
        category_id: null,
        id: this.id
      },
      questions: [],
      datas: [],
      categories: [],
      loading: false
    };
  },
  mounted() {
    this.getData();
  },
  methods: {
    categoryChange() {
      this.getData();
    },
    getData() {
      R.Extentions.paper.PracticeChapter.QuestionsCreate(this.filter).then(resp => {
        this.datas = resp.data.data;
        this.questions = resp.data.questions;
        this.categories = resp.data.categories;
      });
    },
    deleteQuestion(question) {
      R.Extentions.paper.PracticeChapter.QuestionsDelete({ id: this.id, qids: [question.id] }).then(resp => {
        HeyUI.$Message.success('成功');
        this.getData();
      });
    },
    addQuestion(quesiton) {
      R.Extentions.paper.PracticeChapter.QuestionsStore({ id: this.id, qids: [quesiton.id] }).then(resp => {
        HeyUI.$Message.success('成功');
        this.getData();
      });
    }
  }
};
</script>
