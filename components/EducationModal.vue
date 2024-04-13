<template>
  <div>
    <Dialog v-model="isOpen" @close="closeModal">
      <h2 slot="header">Add Education</h2>
      <div slot="body">
        <input v-model="school" placeholder="School" />
        <input v-model="degree" placeholder="Degree" />
        <input v-model="fieldOfStudy" placeholder="Field of Study" />
        <input v-model="startDate" type="date" placeholder="Start Date" />
        <input v-model="endDate" type="date" placeholder="End Date" />
      </div>
      <button slot="footer" @click="saveEducation">Save</button>
    </Dialog>

    <Dialog>
      <DialogTrigger>
        <button @click="openEditProfileDialog">Edit Profile</button>
      </DialogTrigger>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>Edit profile</DialogTitle>
          <DialogDescription>
            Make changes to your profile here. Click save when you're done.
          </DialogDescription>
        </DialogHeader>
        <DialogFooter>
          <button @click="saveProfileChanges">Save changes</button>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  </div>
</template>

<script>
// import { ref } from "@nuxtjs/composition-api";

export default {
  setup() {
    const isOpen = ref(false);
    const school = ref("");
    const degree = ref("");
    const fieldOfStudy = ref("");
    const startDate = ref("");
    const endDate = ref("");

    const openEditProfileDialog = () => {
      // Logic to open the edit profile dialog
    };

    const closeModal = () => {
      isOpen.value = false;
    };

    const saveEducation = () => {
      // Logic to save education data
      closeModal();
    };

    const saveProfileChanges = () => {
      // Logic to save profile changes
    };

    return {
      isOpen,
      school,
      degree,
      fieldOfStudy,
      startDate,
      endDate,
      openEditProfileDialog,
      closeModal,
      saveEducation,
      saveProfileChanges,
    };
  },
};
</script>
