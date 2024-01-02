<template>
  <div>

    <base-header class="pb-7 pt-5 bg-gradient-warning">
      <b-card>
        <b-row>
          <b-col>
            <h2 class="mt-2">Relatórios</h2>
          </b-col>
        </b-row>
      </b-card>
    </base-header>
    <b-container fluid class="mt--7">
      <!--Tables-->
      <b-row class="mt-5">
        <b-col xl="12" class="mb-5 mb-xl-0">
          <books-by-author></books-by-author>
        </b-col>
      </b-row>
      <!--End tables-->
    </b-container>

  </div>
</template>
<script>
  import BooksByAuthor from './BooksByAuthor';

  export default {
    components: {
      BooksByAuthor
    }
  };
</script>
<style>
.el-table .cell{
  padding-left: 0px;
  padding-right: 0px;
}
</style>
