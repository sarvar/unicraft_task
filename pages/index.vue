<template>
  <div>
    <div class="container">
      <h1>User Profile</h1>
      <button @click="openEducationForm">Add Education</button>
      <div v-if="educations.length > 0">
        <div v-for="(education, index) in educations" :key="index">
          <p>{{ education.school }}</p>
          <p>{{ education.degree }}</p>
          <p>{{ education.fieldOfStudy }}</p>
          <p>{{ education.startDate }} - {{ education.endDate }}</p>
          <button @click="removeEducation(index)">Remove</button>
        </div>
      </div>
      <div v-else>
        <p>No data</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      educations: [],
    };
  },
  methods: {
    async openEducationForm() {},
    async fetchEducations() {
      try {
        const response = await fetch("@/static/educations.json");
        const data = await response.json();
        this.educations = data;
      } catch (error) {
        console.error("Error fetching educations:", error);
      }
    },
    async removeEducation(index) {
      this.educations.splice(index, 1);
      await this.saveToJsonFile();
    },
    async saveToJsonFile() {
      try {
        await fetch("/api/save-educations", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(this.educations),
        });
        console.log("Data saved to JSON file successfully.");
      } catch (error) {
        console.error("Error saving data to JSON file:", error);
      }
    },
  },
  async created() {
    await this.fetchEducations();
  },
};
</script>
