<template>
  <div>
    <div class='country'>
      <select v-model="selectedCountry" class='country-select' @change="getContent">
        <option v-for="option in countryOptions" v-bind:value="option.value">
          {{ option.text }}
        </option>
      </select>
    </div>

    </br>

    <div class="wrapper">
      <prismic-edit-button :documentId="documentId"/>

      <h1 class="title">
        {{ $prismic.richTextAsPlain(fields.title) }}
      </h1>

      <div class="icon-wrapper">
        <prismic-image :field="fields.icon" class="icon"/>
      </div>

      <div id='products'>
        <div v-for="product in fields.products" :key='product.id' class='product-card'>
          <prismic-image :field="product.image" class="product-img"/>
          <div class="product-brand">{{ product.brand_name }}</div>
          <div class="product-name">{{ product.name }}</div>
        </div>
      </div>

      <div class="cta-wrapper">
        <prismic-link :field="fields.ctaLink" class="cta">
          {{ $prismic.richTextAsPlain(fields.ctaText) }}
        </prismic-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      documentId: '',
      fields: {
        title: null,
        ctaLink: null,
        ctaText: null,
        icon: null,
        products: []
      },
      selectedCountry: 'en-GB',
      countryOptions: [
        { text: 'Singapore', value: 'en-GB' },
        { text: 'Thailand', value: 'th' }
      ]
    }
  },
  methods: {
    getContent () {
      this.$prismic.client.query(
        this.$prismic.Predicates.at('document.type', 'home'),
        { lang : this.selectedCountry }
      ).then((response) => {
        if (response) {
          let document = response.results[0]
          console.log(document)
          this.documentId = document.id
          this.fields.title = document.data.title
          this.fields.ctaLink = document.data.cta_link
          this.fields.ctaText = document.data.cta_text
          this.fields.icon = document.data.icon
          this.fields.products = document.data.products
        } else {
          this.$router.push({ name: 'not-found' })
        }
      });
    }
  },
  created () {
    this.getContent()
  }
}
</script>

<style>
  .country {
    float: right;
    width: 120px;
    margin-top: 15px;
    margin-right: 25px;
  }

  .country-select {
    display: block;
    font-size: 16px;
    font-family: sans-serif;
    font-weight: 700;
    color: #444;
    line-height: 1.3;
    padding: .6em 1.4em .5em .8em;
    width: 100%;
    max-width: 100%;
    box-sizing: border-box;
    margin: 0;
    border: 1px solid #aaa;
    box-shadow: 0 1px 0 1px rgba(0,0,0,.04);
    border-radius: .5em;
    -moz-appearance: none;
    -webkit-appearance: none;
    appearance: none;
    background-color: #fff;
    background-image: url('data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22292.4%22%20height%3D%22292.4%22%3E%3Cpath%20fill%3D%22%23007CB2%22%20d%3D%22M287%2069.4a17.6%2017.6%200%200%200-13-5.4H18.4c-5%200-9.3%201.8-12.9%205.4A17.6%2017.6%200%200%200%200%2082.2c0%205%201.8%209.3%205.4%2012.9l128%20127.9c3.6%203.6%207.8%205.4%2012.8%205.4s9.2-1.8%2012.8-5.4L287%2095c3.5-3.5%205.4-7.8%205.4-12.8%200-5-1.9-9.2-5.5-12.8z%22%2F%3E%3C%2Fsvg%3E'),
    linear-gradient(to bottom, #ffffff 0%,#e5e5e5 100%);
    background-repeat: no-repeat, repeat;
    background-position: right .7em top 50%, 0 0;
    background-size: .65em auto, 100%;
  }

  .country-select::-ms-expand {
    display: none;
  }

  .country-select:hover {
    border-color: #888;
  }

  .country-select:focus {
    border-color: #aaa;
    box-shadow: 0 0 1px 3px rgba(59, 153, 252, .7);
    box-shadow: 0 0 0 3px -moz-mac-focusring;
    color: #222;
    outline: none;
  }

  .country-select option {
    font-weight:normal;
  }

  .wrapper {
    text-align: center;
    max-width: 820px;
    margin-left: auto;
    margin-right: auto;
    padding: 40px 10px;
    font-family: Avenir, "Helvetica Neue", Helvetica, Arial, sans-serif;
  }

  .title {
    font-size: 32px;
    text-align: center;
  }

  .cta-wrapper {
    margin-top: 40px;
  }

  .cta {
    display: inline-flex;
    justify-content: center;
    align-items: center;
    height: 40px;
    padding: 0 20px;
    background-color: #d50032;
    color: white;
    font-weight: 700;
    font-size: 16px;
  }

  .icon-wrapper {
    margin-top: 40px;
    margin-bottom: 40px;
  }

  .icon {
    max-width: 100%;
  }

  #products {
    max-width: 1200px;
    margin: 0 auto;
    overflow: auto;
  }

  #products::after {
    content: "";
    clear: both;
    display: table;
  }

  .product-card {
    float: left;
    width: 25%;
    box-sizing: border-box;
    padding: 10px;
  }

  .product-img {
    width: 100%;
    height: auto;
  }

  .product-brand {
    font-weight: 600;
    font-size: 14px;
    margin-top: 20px;
  }

  .product-name {
    font-weight: 400;
    font-size: 14px;
    line-height: 20px;
  }

  /* Responsive */
  @media only screen and (max-width: 1024px) {
    div.product-card { width: 33%; }
  }
  @media only screen and (max-width: 600px) {
    div.product-card { width: 50%; }
  }
</style>
