<template>
  <div class="userinfo">
    <div>
      <h1>
        Аутентификация успешна
      </h1>
      <h2>
        Сценарий 3: Прямой вход через Zitadel
      </h2>

      <!-- Ключевые JWT claims для eFlow -->
      <div v-if="user" class="key-claims">
        <h3>Ключевые claims для eFlow:</h3>
        <div class="claim-item">
          <strong>prid</strong> (Project Resource ID): <code>{{ user.prid || 'не установлен' }}</code>
        </div>
        <div class="claim-item">
          <strong>scp</strong> (Scopes/Roles):
          <code v-if="Array.isArray(user.scp)">{{ user.scp.join(', ') }}</code>
          <code v-else>{{ user.scp || 'не установлен' }}</code>
        </div>
        <div class="claim-item">
          <strong>x_user_name</strong> (Username): <code>{{ user.x_user_name || 'не установлен' }}</code>
        </div>
      </div>

      <!-- Все остальные claims -->
      <details open>
        <summary><h3 style="display: inline-block; cursor: pointer;">Все JWT claims (расширенные данные от Zitadel userinfo endpoint)</h3></summary>
        <ul class="claims">
          <li
            v-for="c in claims"
            :key="c.key"
          >
            <strong>{{ c.key }}</strong>:
            <code v-if="Array.isArray(c.value)">{{ JSON.stringify(c.value) }}</code>
            <code v-else-if="typeof c.value === 'object'">{{ JSON.stringify(c.value, null, 2) }}</code>
            <code v-else>{{ c.value }}</code>
          </li>
        </ul>
      </details>
    </div>
  </div>
</template>

<script lang="ts">
import {defineComponent} from "vue";

export default defineComponent({
  computed: {
    user() {
      return this.$zitadel.oidcAuth.userProfile
    },
    claims() {
      if (this.user) {
        return Object.keys(this.user).map(key => ({
          key,
          value: this.user[key]
        }))
      }
      return []
    }
  }
});
</script>
<style scoped>
.key-claims {
  background-color: #f5f5f5;
  border-left: 4px solid #42b983;
  padding: 1rem;
  margin: 2rem 0;
}

.claim-item {
  margin: 0.5rem 0;
  padding: 0.5rem;
  background-color: white;
  border-radius: 4px;
}

.claim-item code {
  background-color: #f0f0f0;
  padding: 0.2rem 0.5rem;
  border-radius: 3px;
  font-family: 'Courier New', monospace;
  color: #e74c3c;
}

details {
  margin-top: 2rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 1rem;
}

summary {
  cursor: pointer;
  font-weight: bold;
}

.claims {
  list-style: none;
  padding: 0;
  margin-top: 1rem;
}

.claims li {
  padding: 0.5rem;
  margin: 0.5rem 0;
  background-color: #fafafa;
  border-radius: 4px;
  word-break: break-all;
}

.claims code {
  display: inline-block;
  background-color: #f0f0f0;
  padding: 0.2rem 0.5rem;
  border-radius: 3px;
  font-family: 'Courier New', monospace;
  color: #2c3e50;
  margin-left: 0.5rem;
}

@media (min-width: 1024px) {
  .userinfo {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>