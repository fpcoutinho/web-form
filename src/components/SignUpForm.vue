<template>
  <form @submit.prevent="handleSubmit">
    <div class="form-group">
      <label for="email">Email</label>
      <input type="email" id="email" v-model="email" required />
      <div class="error" v-if="!isValidEmail && !(email == '')">
        Email is {{ isValidEmail ? "valid" : "invalid" }}
      </div>
    </div>
    <div class="form-group">
      <label for="password">Password</label>
      <input type="password" id="password" v-model="password" required />
      <div class="error" v-if="!isValidPassword && !(password == '')">
        {{
          isValidPassword ? "" : "Password must be at least 8 characters long"
        }}
      </div>
    </div>
    <div class="form-group">
      <label for="role">Role</label>
      <select id="role" v-model="role">
        <option value="developer">Developer</option>
        <option value="designer">Designer</option>
      </select>
    </div>
    <div class="form-group">
      <label for="skills">Skills</label>
      <input type="text" id="skills" v-model="tempSkill" @keyup="addSkill" />
      <div v-for="skill in skills" class="pill" @click="removeSkill">
        {{ skill }}
      </div>
    </div>
    <div class="form-group">
      <label for="terms">
        <input type="checkbox" id="terms" v-model="terms" />
        I agree to the terms and conditions
      </label>
    </div>
    <button
      type="submit"
      :disabled="!isValidEmail || !isValidPassword || !terms"
    >
      Sign Up
    </button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      email: "",
      password: "",
      role: "",
      terms: false,
      tempSkill: "",
      skills: [],
    }
  },
  methods: {
    handleSubmit() {
      if (this.isValidEmail || this.isValidPassword || this.terms) {
        console.log({
          email: this.email,
          password: this.password,
          role: this.role,
          skills: this.skills,
        })
      }
    },
    addSkill(e) {
      if (e.key == "," && this.tempSkill.slice(0, -1)) {
        if (this.tempSkill.endsWith(",")) {
          this.tempSkill = this.tempSkill.slice(0, -1)
        }
        if (
          !this.skills
            .map((str) => str.toLowerCase())
            .includes(this.tempSkill.toLowerCase())
        ) {
          this.skills.push(this.tempSkill)
        }
        this.tempSkill = ""
      }
    },
    removeSkill(e) {
      this.skills = this.skills.filter((skill) => skill != e.target.innerText)
    },
  },
  computed: {
    isValidEmail() {
      return /^[^@]+@\w+(\.\w+)+\w$/.test(this.email)
    },
    isValidPassword() {
      return this.password.length >= 8
    },
  },
}
</script>

<style>
form {
  max-width: 420px;
  margin: 30px auto;
  background: white;
  text-align: left;
  padding: 40px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
}
label {
  color: #aaa;
  display: inline-block;
  margin: 25px 0 15px;
  font-size: 0.6em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}
input,
select {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid #ddd;
  color: #555;
}
input[type="checkbox"] {
  display: inline-block;
  width: 16px;
  margin: 0 10px 0 0;
  position: relative;
  top: 2px;
}
.pill {
  display: inline-block;
  margin: 20px 10px 0 0;
  padding: 6px 12px;
  background: #eee;
  border-radius: 20px;
  font-size: 12px;
  letter-spacing: 1px;
  font-weight: bold;
  color: #777;
  cursor: pointer;
}
button {
  background: #0b6dff;
  border: 0;
  padding: 10px 20px;
  margin-top: 20px;
  color: white;
  border-radius: 20px;
  cursor: pointer;
}
button:disabled {
  background: #ccc;
  cursor: not-allowed;
}
.submit {
  text-align: center;
}
.error {
  color: #ff0062;
  margin-top: 10px;
  font-size: 0.8em;
  font-weight: bold;
}
</style>
