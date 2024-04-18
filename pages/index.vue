<template>
  <div>
    <div class="container">
      <div class="text-center py-10">
        <h1 class="text-2xl">User Profile</h1>
      </div>
      <div class="mb-5">
        <Dialog :open="isDialogOpen.value">
          <DialogTrigger><Button>Add new education</Button> </DialogTrigger>
          <DialogContent>
            <DialogHeader class="mb-3">
              <DialogTitle><h2>Add education</h2></DialogTitle>
            </DialogHeader>
            <FormField>
              <FormItem class="mb-5 block">
                <FormLabel class="block mb-1 text-slate-500 text-sm"
                  >School</FormLabel
                >
                <FormControl class="w-full">
                  <Input
                    v-model="newEducation.school"
                    class="px-2 h-[32px] w-full border border-slate-900 rounded-sm text-sm"
                    type="text"
                    placeholder="Ex: Boston University"
                  />
                </FormControl>
              </FormItem>
              <FormItem class="mb-5 block">
                <FormLabel class="block mb-1 text-slate-500 text-sm"
                  >Degree</FormLabel
                >
                <FormControl class="w-full">
                  <Input
                    v-model="newEducation.degree"
                    class="px-2 h-[32px] w-full border border-slate-900 rounded-sm text-sm"
                    type="text"
                    placeholder="Ex: Bachelor's"
                  />
                </FormControl>
              </FormItem>
              <FormItem class="mb-5 block">
                <FormLabel class="block mb-1 text-slate-500 text-sm"
                  >Field of study</FormLabel
                >
                <FormControl class="w-full">
                  <Input
                    class="px-2 h-[32px] w-full border border-slate-900 rounded-sm text-sm"
                    type="text"
                    placeholder="Ex: Business"
                  />
                </FormControl>
              </FormItem>
              <!-- Start Date Select -->
              <FormItem class="mb-5 block">
                <FormLabel class="block mb-1 text-slate-500 text-sm"
                  >Start date</FormLabel
                >
                <div class="grid grid-cols-2 gap-4">
                  <FormControl class="w-full">
                    <Select v-model="newEducation.startDateMonth">
                      <SelectTrigger>
                        <SelectValue :placeholder="startDatePlaceholder" />
                      </SelectTrigger>
                      <SelectContent>
                        <SelectGroup>
                          <template v-for="option in months" :key="option">
                            <SelectItem :value="option">{{
                              option
                            }}</SelectItem>
                          </template>
                        </SelectGroup>
                      </SelectContent>
                    </Select>
                  </FormControl>
                  <FormControl class="w-full">
                    <Select v-model="newEducation.startDateYear">
                      <SelectTrigger>
                        <SelectValue :placeholder="yearPlaceholder" />
                      </SelectTrigger>
                      <SelectContent>
                        <SelectGroup>
                          <template v-for="option in years" :key="option">
                            <SelectItem :value="option">{{
                              option
                            }}</SelectItem>
                          </template>
                        </SelectGroup>
                      </SelectContent>
                    </Select>
                  </FormControl>
                </div>
              </FormItem>
              <!-- End Date Select -->
              <FormItem class="mb-5 block">
                <FormLabel class="block mb-1 text-slate-500 text-sm"
                  >End date (or expected)</FormLabel
                >
                <div class="grid grid-cols-2 gap-4">
                  <FormControl class="w-full">
                    <Select v-model="newEducation.endDateMonth">
                      <SelectTrigger>
                        <SelectValue :placeholder="endDatePlaceholder" />
                      </SelectTrigger>
                      <SelectContent>
                        <SelectGroup>
                          <template v-for="option in months" :key="option">
                            <SelectItem :value="option">{{
                              option
                            }}</SelectItem>
                          </template>
                        </SelectGroup>
                      </SelectContent>
                    </Select>
                  </FormControl>
                  <FormControl class="w-full">
                    <Select v-model="newEducation.endDateYear">
                      <SelectTrigger>
                        <SelectValue :placeholder="yearPlaceholder" />
                      </SelectTrigger>
                      <SelectContent>
                        <SelectGroup>
                          <template v-for="option in years" :key="option">
                            <SelectItem :value="option">{{
                              option
                            }}</SelectItem>
                          </template>
                        </SelectGroup>
                      </SelectContent>
                    </Select>
                  </FormControl>
                </div>
              </FormItem>
              <FormItem class="mb-5 block">
                <FormLabel class="block mb-1 text-slate-500 text-sm"
                  >Grade</FormLabel
                >
                <FormControl class="w-full">
                  <Input
                    class="px-2 h-[32px] w-full border border-slate-900 rounded-sm text-sm"
                    type="text"
                    placeholder=""
                  />
                </FormControl>
              </FormItem>
              <FormItem class="mb-5 block">
                <FormLabel class="block mb-1 text-slate-500 text-sm"
                  >Activities and societies</FormLabel
                >
                <FormControl class="w-full">
                  <Textarea
                    class="px-2 w-full border border-slate-900 rounded-sm text-sm"
                    placeholder="Ex: Alpha Phi Omega, Marching Band, Volleyball"
                  />
                </FormControl>
              </FormItem>
              <FormItem class="mb-5 block">
                <FormLabel class="block mb-1 text-slate-500 text-sm"
                  >Description</FormLabel
                >
                <FormControl class="w-full">
                  <Textarea
                    class="px-2 w-full border border-slate-900 rounded-sm text-sm"
                  />
                </FormControl>
              </FormItem>
            </FormField>

            <DialogFooter>
              <Button @click="saveEducationItem">Save</Button>
            </DialogFooter>
          </DialogContent>
        </Dialog>
      </div>
      <div class="w-full">
        <Table v-if="educationItems.length > 0">
          <TableHeader>
            <TableRow>
              <TableHead class="w-[100px]">№</TableHead>
              <TableHead>Name</TableHead>
              <TableHead class="w-[100px]">Action</TableHead>
            </TableRow>
          </TableHeader>
          <TableBody>
            <TableRow v-for="(item, index) in educationItems" :key="index">
              <TableCell class="font-medium">{{ index + 1 }}</TableCell>
              <TableCell>{{ item.school }}</TableCell>
              <TableCell
                ><Button @click="removeEducationItem(index)"
                  >Remove</Button
                ></TableCell
              >
            </TableRow>
          </TableBody>
        </Table>
        <div v-else class="py-3 text-center bg-slate-200 rounded-md">
          <p>No data</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectLabel,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select";

import {
  Table,
  TableBody,
  TableCaption,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table";

const educationItems = ref([]);
const isDialogOpen = ref(false);

const months = [
  "January",
  "February",
  "March",
  "April",
  "May",
  "June",
  "July",
  "August",
  "September",
  "October",
  "November",
  "December",
];
const years = Array.from({ length: 10 }, (_, i) => String(2024 - i));

const startDatePlaceholder = "Start Date";
const endDatePlaceholder = "End Date";
const yearPlaceholder = "Year";

const newEducation = ref({
  school: "",
  degree: "",
  fieldOfStudy: "",
  startDateMonth: "",
  startDateYear: "",
  endDateMonth: "",
  endDateYear: "",
  grade: "",
  activities: "",
  description: "",
});

const formData = ref({
  school: "",
  degree: "",
  fieldOfStudy: "",
  startDateMonth: "",
  startDateYear: "",
  endDateMonth: "",
  endDateYear: "",
  grade: "",
  activities: "",
  description: "",
});

const closeDialog = () => {
  isDialogOpen.value = false;
  clearFormData();
};

const saveEducationItem = () => {
  console.log("Before pushing new item:", educationItems.value);
  const newItem = { ...formData.value };
  educationItems.value.push(newItem);
  console.log("After pushing new item:", educationItems.value);
  clearFormData();
  isDialogOpen.value = false;
};

const removeEducationItem = (index) => {
  educationItems.value.splice(index, 1);
};

const clearFormData = () => {
  console.log("Clearing form data:", formData.value);
  formData.value = {
    school: "",
    degree: "",
    fieldOfStudy: "",
    startDateMonth: "",
    startDateYear: "",
    endDateMonth: "",
    endDateYear: "",
    grade: "",
    activities: "",
    description: "",
  };
};
</script>
