<template>
  <section
    class="c-card"
    :data-coin-code="$props.coin.code"
    :data-card-animation="$data.animation"
  >
    <header class="header">
      <h3 class="title">{{ $props.coin.name }}</h3>
      <hr class="divider" />
    </header>

    <main class="content">
      <c-field
        v-model="$data.value"
        :newValue="$data.newValue"
        :name="$props.coin.code"
        @blur="updateCardAnimation({ status: false })"
        @focus="updateCardAnimation({ status: true })"
      >
        <template #prefix>{{ $props.coin.symbol }}</template>
        Valor em {{ $props.coin.code }}
      </c-field>

      <aside class="details">
        <c-icon name="up" text>{{ coinUp }}</c-icon>
        <c-icon name="down" text>{{ coinDown }}</c-icon>
        <c-icon name="variation" text>{{ coinUp }}</c-icon>
      </aside>
    </main>

    <footer class="footer">
      <hr class="divider" />
      <p class="date" v-text="$props.coin.create_date"></p>
    </footer>
  </section>
</template>

<script>
import Icon from "@/components/Icon";
import Field from "@/components/Field";

export default {
  name: "Card",
  components: {
    "c-icon": Icon,
    "c-field": Field,
  },
  props: {
    coin: {
      type: Object,
      required: true,
    },
    brl: {
      type: Number,
      required: true,
    },
  },
  data: () => ({
    value: "",
    newValue: "",
    animation: false,
  }),
  computed: {
    coinUp() {
      return this.formatCoinValue(this.$data.value * this.$props.coin.high);
    },
    coinDown() {
      return this.formatCoinValue(this.$data.value * this.$props.coin.low);
    },
    coinVar() {
      return this.formatCoinValue(this.$data.value * this.$props.coin.diff);
    },
  },
  watch: {
    brl(value) {
      this.$data.value = (value / this.$props.coin.medium).toFixed(2);
      this.$data.newValue = this.$data.value;
    },
  },
  methods: {
    updateCardAnimation({ status }) {
      this.$data.animation = status;
    },
    formatCoinValue(value) {
      return globalThis.Intl.NumberFormat("pt-BR", {
        style: "currency",
        currency: "BRL",
      }).format(value);
    },
  },
};
</script>

<style lang="css">
.c-card {
  --position: 45deg;

  display: flex;
  position: relative;

  flex-direction: column;
  justify-content: space-around;

  width: 280px;
  height: 340px;
  min-width: 260px;

  padding: 6px 24px;
  border-radius: 8px;

  color: #fff;
  background-color: var(--bg-secondary);
}

.c-card::before {
  content: "";
  position: absolute;

  top: -4px;
  right: -4px;
  bottom: -4px;
  left: -4px;

  z-index: -1;
}

.c-card::after {
  content: "";
  position: absolute;

  top: -8px;
  right: -8px;
  bottom: -8px;
  left: -8px;

  z-index: -2;
  filter: blur(6px);
}

.c-card::before,
.c-card::after {
  animation: card-animation 1.6s linear infinite paused;
  border-radius: 6px;

  background: #fff
    linear-gradient(
      var(--position),
      var(--primary),
      var(--bg-primary),
      var(--secondary)
    );
}

.c-card[data-card-animation="true"]::before,
.c-card[data-card-animation="true"]::after {
  animation-play-state: running;
}

.c-card .divider {
  height: 2px;
}

.c-card > .header .title {
  padding: 12px;

  text-align: center;
  text-shadow: 0 0 4px #000;

  color: #fff;
  font-size: 1.4rem;
  font-weight: 700;
}

.c-card > .content {
  padding: 24px 0;
  text-shadow: 0 0 2px #000;

  font-size: 1rem;
  font-weight: 500;
}

.c-card > .content .details {
  margin-top: 24px;
}

.c-card > .footer .date {
  padding: 12px 0;

  text-align: center;
  text-shadow: 0 0 2px #000;

  font-size: 1rem;
  font-weight: 300;
}

@keyframes card-animation {
  0% {
    --position: 45deg;
  }
  5% {
    --position: 63deg;
  }
  10% {
    --position: 81deg;
  }
  15% {
    --position: 99deg;
  }
  20% {
    --position: 117deg;
  }
  25% {
    --position: 135deg;
  }
  30% {
    --position: 153deg;
  }
  35% {
    --position: 171deg;
  }
  40% {
    --position: 189deg;
  }
  45% {
    --position: 207deg;
  }
  50% {
    --position: 225deg;
  }
  55% {
    --position: 243deg;
  }
  60% {
    --position: 261deg;
  }
  65% {
    --position: 279deg;
  }
  70% {
    --position: 297deg;
  }
  75% {
    --position: 315deg;
  }
  80% {
    --position: 333deg;
  }
  85% {
    --position: 351deg;
  }
  90% {
    --position: 9deg;
  }
  95% {
    --position: 27deg;
  }
  100% {
    --position: 45deg;
  }
}
</style>
