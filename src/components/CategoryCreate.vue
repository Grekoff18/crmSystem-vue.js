<template>
  <div class="col s12 m6">
    <div>
      <div class="page-subtitle">
        <h4>Create</h4>
      </div>

      <form @submit.prevent="submitHandler">
        <div class="input-field">
          <input
            id="name"
            type="text"
            v-model="title"
            :class="{invalid: $v.title.$dirty && !$v.title.required}"
          >
          <label for="name">Name</label>
          <span
            class="helper-text invalid"
            v-if="$v.title.$dirty && !$v.title.required"
          >
            Enter category name
          </span>
        </div>

        <div class="input-field">
          <input
            id="limit"
            type="number"
            v-model.number="limit"
            :class="{invalid: $v.limit.$dirty && !$v.limit.minValue}"
          >
          <label for="limit">Limit</label>
          <span
            class="helper-text invalid"
            v-if="$v.limit.$dirty && !$v.limit.minValue"
          >
            Minimal limit -> {{$v.limit.$params.minValue.min}}
          </span>
        </div>

        <button class="btn waves-effect waves-light" type="submit">
          Create
          <i class="material-icons right">send</i>
        </button>
      </form>
    </div>
  </div>
</template>

<script>
// Importing different plugins for validating input fields
import {required, minValue} from 'vuelidate/lib/validators'

export default {
  name: "CategoryCreate",

  data() {
    return {
      title: "",
      limit: null,
    }
  },

	// Set up for validating our input fields
  validations: {
    title: {required},
    limit: {minValue: minValue(100)}
  },

  // Fix a bug with animation in the limit field
  mounted() {
    M.updateTextFields()
  },

  methods: {
    // Form validate
    async submitHandler() {
      if (this.$v.$invalid) {
        this.$v.$touch()
        return
      }

      try {
				// Send some info to store for create new category
        const category = await this.$store.dispatch("createCategory", {
          title: this.title,
          limit: this.limit,
				})

				// After sending set to zero other info about category in fields
        this.title = ""
        this.limit = this.$v.limit.$params.minValue.min
        this.$message("Category created")
				this.$emit("created", category)
				// Reset all validation warnings 
				this.$v.reset()
      } catch (e) {}
    },
  },
}
</script>