<template>
  <div class="w-full">
    <!-- Table -->
    <div class="rounded-md border">
      <Table>
        <TableHeader>
          <!-- Table headers -->
          <TableRow
            v-for="headerGroup in table.getHeaderGroups()"
            :key="headerGroup.id"
          >
            <TableHead
              v-for="(header, index) in headerGroup.headers"
              :key="header.id"
              :class="{
                'sticky bg-background/95': header.column.getIsPinned(),
                'left-0': header.column.getIsPinned() === 'left',
                'right-0': header.column.getIsPinned() === 'right',
              }"
              :style="{ width: headerWidths[index] }"
            >
              <FlexRender
                v-if="!header.isPlaceholder"
                :render="header.column.columnDef.header"
                :props="header.getContext()"
              />
            </TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          <!-- Table rows -->
          <template v-if="table.getRowModel().rows?.length">
            <TableRow
              v-for="row in table.getRowModel().rows"
              :key="row.id"
              :data-state="row.getIsSelected() && 'selected'"
            >
              <!-- Table cells -->
              <TableCell
                v-for="cell in row.getVisibleCells()"
                :key="cell.id"
                :class="{
                  'sticky bg-background/95': cell.column.getIsPinned(),
                  'left-0': cell.column.getIsPinned() === 'left',
                  'right-0': cell.column.getIsPinned() === 'right',
                }"
              >
                <FlexRender
                  :render="cell.column.columnDef.cell"
                  :props="cell.getContext()"
                />
              </TableCell>
              <!-- Action column with Remove button -->
              <TableCell>
                <Button @click="removeRow(row)">Remove</Button>
              </TableCell>
            </TableRow>
          </template>
          <!-- Message for no results -->
          <TableRow v-else>
            <TableCell :colspan="columns.length" class="h-24 text-center">
              No results.
            </TableCell>
          </TableRow>
        </TableBody>
      </Table>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { ColumnFiltersState, VisibilityState } from "@tanstack/vue-table";
import {
  FlexRender,
  createColumnHelper,
  getCoreRowModel,
  getFilteredRowModel,
  getPaginationRowModel,
  getSortedRowModel,
  useVueTable,
} from "@tanstack/vue-table";
import { Input } from "@/components/ui/input";
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table";

// Define your Payment interface
interface Payment {
  id: string;
  name: string;
  // Add more fields as needed
}

// Sample payment data
const data: Payment[] = [
  {
    id: "1",
    name: "John",
  },
  // Add more sample data as needed
];

// Define your column helper
const columnHelper = createColumnHelper<Payment>();

// Define your columns
const columns = [
  columnHelper.accessor("id", {
    header: "ID",
    cell: ({ row }) => row.getValue("id"),
  }),
  columnHelper.accessor("name", {
    header: "Name",
    cell: ({ row }) => row.getValue("name"),
  }),
];

// Define your column filters and visibility states
const columnFilters = ref<ColumnFiltersState>([]);
const columnVisibility = ref<VisibilityState>({});

// Create the table instance
const table = useVueTable({
  data,
  columns,
  getCoreRowModel: getCoreRowModel(),
  getPaginationRowModel: getPaginationRowModel(),
  getSortedRowModel: getSortedRowModel(),
  getFilteredRowModel: getFilteredRowModel(),
  state: {
    get columnFilters() {
      return columnFilters.value;
    },
    get columnVisibility() {
      return columnVisibility.value;
    },
  },
});

// Method to remove a row from the table
function removeRow(row) {
  const rowIndex = table.getRowIndex(row);
  if (rowIndex !== -1) {
    table.removeRow(rowIndex);
  }
}

// Header widths
const headerWidths = ref<string[]>(["10%", "80%", "10%"]);
</script>

<style scoped>
/* Your scoped styles here */
</style>
