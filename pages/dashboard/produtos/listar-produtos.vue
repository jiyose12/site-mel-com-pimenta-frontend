<template>
  <b-card>
    <div slot="header">
      <strong>Listagem</strong> de Produtos

      	<b-input-group size="lg" class="mt-3 mb-3">
          <b-form-input
            v-model="keyword"
            placeholder="Pesquisar Produto por Nome ou Preço"
            type="text"
            >
          </b-form-input>
          <b-input-group-text class="input-text" slot="append">
            <b-button
            :disabled="!keyword"
            variant="link"
            size="lg"
            @click="keyword = ''"
            class="p-0">
              <i class="fa fa-remove">
              </i>
            </b-button>
          </b-input-group-text>
        </b-input-group>

      <b-table responsive striped hover :keyword="keyword" :items="items" :fields="fields">
        <div slot="HEAD[]" class="text-nowrap" slot-scope="scope">{{ scope.label }}</div>
        <div slot="[]" class="text-nowrap" slot-scope="scope">{{ scope.value }}</div>

        <template slot="[actions]" slot-scope="props">
          <div class="product-actions">
            <b-button variant="info" class="ui basic button"
              @click="onAction('view-item', props.item)">
              <i class="fa fa-search"></i>
            </b-button>
            <b-button variant="warning" class="ui basic button"
              @click="onAction('edit-item', props.item)">
              <i class="fa fa-edit"></i>
            </b-button>
            <b-button variant="danger" class="ui basic button"
              @click="onAction('delete-item', props.item)">
              <i class="fa fa-trash"></i>
            </b-button>
          </div>
        </template>

      </b-table>
    </div>
  </b-card>
</template>

<script>

export default {
  computed: {
    items () {
			return this.keyword
        ? this.productData.filter(item => item.gross_price.includes(this.keyword)
        || item.name.includes(this.keyword)
        // || item.color !== null || item.color.includes(this.keyword)
        // || item.flavor ? item.flavor.includes(this.keyword) : item.flavor == ' '
        // || item.size.includes(this.keyword)
        )
				: this.productData
    },
    fields() {
      return [
        {
          key: 'name',
          label: 'Nome',
          sortable: true
        },
        {
          key: 'gross_price',
          label: 'Preço Bruto',
          sortable: true
        },
        {
          key: 'discount',
          label: 'Desconto %',
          sortable: true
        },
        {
          key: 'color',
          label: 'Cor',
          sortable: false
        },
        {
          key: 'flavor',
          label: 'Sabor',
          sortable: false
        },
        {
          key: 'size',
          label: 'tamanho',
          sortable: false
        },
        {
          key: 'actions',
          label: 'Ações'
        }
        // {
        //   key: 'age',
        //   label: 'Person age',
        //   sortable: true,
        //   // Variant applies to the whole column, including the header and footer
        //   variant: 'danger'
        // }
      ]
    }
  },
  data() {
    return {
        productData: null,
        keyword: ''
    }
  },
  methods: {
    async index () {
      try {
        const { data } = await this.$axios.get('/product')
        this.productData = data
        console.dir(this.productData)
        console.dir(this.items)
      } catch (e) {
        this.error = e.response.data
      }
    },
    onAction (action, data) {
      console.log('slot) action: ' + action, typeof data.color)
    }
  },
  mounted() {
    this.index();
  }
}
</script>

  <style>
    .product-actions button.ui.button {
      /* padding: 8px 8px; */
      width: 5em;
      height: 3em;
    }
    .product-actions button.ui.button > i.icon {
      /* margin: auto !important; */
    }

    .input-text {
      padding: 0 .5em 0 .5em;
    }
    .fa {
      font-size: 1.5em;
    }
  </style>
