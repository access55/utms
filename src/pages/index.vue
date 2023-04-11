<script setup lang="ts">
import Clipboard from 'clipboard'

defineOptions({
  name: 'IndexPage',
})

const websites = [
  'https://www.a55.tech/smart-capital/estoque',
  'https://www.a55.tech/smart-capital/antecipacao',
  'https://www.a55.tech/smart-capital/conta-digital',
  'https://www.a55.tech/smart-capital/investimento',
  'https://www.a55.tech/solucoes/para-investidores/infraestrutura-de-capitais',
  'https://www.a55.tech/solucoes/para-franquias/franqueados',
  'https://www.a55.tech/solucoes/para-parceiros/embedded-finance',
]

const sourceMap: any = {
  // utm_source: 'utm_channel'
  direct: 'marketing',
  organic: 'marketing',
  blog: 'marketing',
  google_ads: 'marketing',
  facebook_ads: 'marketing',
  linkedin_ads: 'marketing',
  facebook: 'marketing',
  instagram: 'marketing',
  linkedin: 'marketing',
  twitter: 'marketing',
  email_marketing: 'marketing',
  hiperlink: 'marketing',
  co_marketing: 'marketing',
  prospection: 'outbound',
  indication: 'outbound',
  relationship: 'outbound',
  communities: 'partners',
  integration_partner: 'partners',
  white_label: 'partners',
  referral: 'partners',
}

const sources = Object.keys(sourceMap)

const website = $ref('https://www.a55.tech/smart-capital/estoque')
const source = $ref('direct')
const campaign = $ref('') // texto livre
const copyText = ref('COPIAR')

const utms = computed(() => {
  const map = {
    source,
    channel: sourceMap[source],
    campaign,
    content: website.split('/').pop(),
  }
  return map
})

const result = computed(() => {
  const encode = (s: string) => encodeURIComponent(s)
  let res = website
  if (utms.value.campaign)
    res = res.concat('?utm_campaign=', encode(utms.value.campaign))

  if (utms.value.source !== 'direct')
    res = res.concat('&utm_source=', encode(utms.value.source))

  return res
})

onMounted(() => {
  const cb = new Clipboard('.copy-btn', {
    text: () => result.value,
  })
  cb.on('success', (e) => {
    e.clearSelection()
    copyText.value = 'COPIADO!!!'
    setTimeout(() => {
      copyText.value = 'COPIAR'
    }, 800)
  })
})
</script>

<template>
  <div>
    <h1 text-xl>
      A55 UTM Tool
    </h1>
    <p>
      <em text-sm op75>Gerador de links com UTMs</em>
    </p>
    <hr mt-5 mb-4>
    <form class="utm-form" @submit.prevent>
      <div>
        <label for="website">Website URL <span color-red>*</span></label>
        <TheSelect
          v-model="website"
          :options="websites"
          placeholder="website?"
          autocomplete="false"
        />
      </div>

      <div>
        <label for="source">
          UTM Source <span color-red>*</span>
        </label>
        <TheSelect
          v-model="source"
          :options="sources"
          placeholder="utm_source"
          autocomplete="false"
        />
      </div>

      <div>
        <label for="source">
          UTM Campaign
        </label>
        <TheInput
          v-model="campaign"
          placeholder="utm_campaign"
          autocomplete="false"
        />
      </div>
    </form>
    <hr mt-5 mb-4>
    <section>
      <h2>
        URL final:
      </h2>
      <code b-2 p-4 block w-fit my-4>
        {{ result }}
      </code>
      <button btn class="copy-btn">
        {{ copyText }}
      </button>
    </section>
    <hr mt-5 mb-4>
    <section>
      <h2>
        UTMs ativas:
      </h2>
      <table class="utm-table" my-4>
        <tr>
          <th>utm_channel</th>
          <td>{{ utms.channel }}</td>
        </tr>
        <tr>
          <th>utm_source</th>
          <td>{{ utms.source }}</td>
        </tr>
        <tr>
          <th>utm_content</th>
          <td>{{ utms.content }}</td>
        </tr>
        <tr>
          <th>utm_campaign</th>
          <td>{{ utms.campaign }}</td>
        </tr>
      </table>
    </section>
  </div>
</template>

<style lang="less" scoped>
.utm-form {
  > div {
    display: flex;
    flex-flow: row wrap;
    align-items: center;
    margin-bottom: 2rem;
    width: 100%;
    max-width: 600px;
    > * {
      flex: 1;
    }
    > label {
      flex: 0 130px;
    }
  }
}

.utm-table {
  text-align: left;
  th,td {
    border: 1px solid #ccc;
    padding: .5rem;
  }
}
</style>
