<template>
  <div>
    <form class="form-inline">
      <table>
        <tr>
          <td style="padding: 5px;">产品ID</td>
          <td style="padding: 5px;"><input type="text" v-model="query.name" class="form-control" placeholder="产品ID"></td>
          <td style="padding: 5px;">产品名</td>
          <td style="padding: 5px;"><input type="text" v-model="query.name" class="form-control" placeholder="产品名"></td>
          <td style="padding: 5px;">供应商名</td>
          <td style="padding: 5px;"><input type="text" v-model="query.name" class="form-control" placeholder="供应商名"></td>
        </tr>
        <tr>
          <td style="padding: 5px;">门店名</td>
          <td style="padding: 5px;"><input type="text" v-model="query.name" class="form-control" placeholder="门店名"></td>
          <td style="padding: 5px;">房型名</td>
          <td style="padding: 5px;"><input type="text" v-model="query.name" class="form-control" placeholder="房型名"></td>
          <td style="padding: 5px;"></td>
          <td style="padding: 5px;">
            <button type="button" class="btn btn-secondary" @click="queryData">查询</button>
            <router-link :to="{ name: 'hotelProductAdd', params: { sid: $route.params.sid }}" aria-pressed="true">添加
            </router-link>
          </td>
        </tr>
      </table>
    </form>
    <table class="table">
      <thead>
      <tr>
        <th>产品ID</th>
        <th>供应商SID</th>
        <th>品类树ID</th>
        <th>产品名</th>
        <th>状态</th>
        <th>负责人</th>
        <th>生效日期</th>
        <th>过期日期</th>
        <th>创建时间</th>
        <th>修改时间</th>
        <th>操作</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="item in products">
        <td>
          <router-link :to="{ name: 'hotelProduct', params: { sid: item.sid, id: item.id }}">{{item.id}}</router-link>
        </td>
        <td>{{item.sid}}</td>
        <td>{{item.treeId}}</td>
        <td>{{item.name}}</td>
        <td>{{item.stateText}}</td>
        <td>{{item.owner}}</td>
        <td>{{item.effectDateText}}</td>
        <td>{{item.expireDateText}}</td>
        <td>{{item.createTime | timeAgo}}</td>
        <td>{{item.modifyTime | timeAgo}}</td>
        <td>
          <router-link :to="{ name: 'hotelProductEdit', params: { sid: item.sid, id: item.id }}">修改</router-link>
          <button type="button" class="btn btn-primary btn-xs">生产</button>
          <button type="button" class="btn btn-primary btn-xs" v-show="item.state == 'PASS'">上架</button>
          <button type="button" class="btn btn-primary btn-xs" v-show="item.state == 'PUBLISH'">下架</button>
          <router-link :to="{ name: 'hotelProductSku', params: { sid: item.sid, id: item.id }}">报价</router-link>
        </td>
      </tr>
      </tbody>
    </table>
    <nav aria-label="Page navigation">
      <ul class="pagination">
        <li>
          <a href="#" aria-label="Previous">
            <span aria-hidden="true">&laquo;</span>
          </a>
        </li>
        <li class="active"><a href="#">{{query.pageNow}}<span class="sr-only">(current)</span></a></li>
        <li>
          <a href="#" aria-label="Next">
            <span aria-hidden="true">&raquo;</span>
          </a>
        </li>
        <li>
          <span>共{{totalPage}}页</span>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
  import {listProduct} from './api'

  export default {
    name: 'hotel-supplier-product-list-view',
    data() {
      return {
        query: {pageNow: 1, pageSize: 10, sortKey: ''},
        products: [],
        count: 0
      }
    },
    computed: {
      totalPage () {
        return parseInt(this.count / this.query.pageSize) + 1
      }
    },
    created () {
      this.fetchData()
    },
    methods: {
      fetchData () {
        this.query.sid = this.$route.params.sid
        listProduct(this.query, (body) => {
          this.products = body.data.list
          this.count = body.data.total
        })
      },
      queryData () {
        listProduct(this.query, (body) => {
          this.products = body.data.list
          this.count = body.data.total
        })
      }
    }
  }

</script>
