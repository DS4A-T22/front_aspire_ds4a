<template>
  <div id="app">
    <h3 class="title">Pacientes</h3>
    
    </br>
    <DataTable
      :header-fields="headerFields"
      :sort-field="sortField"
      :sort="sort"
      :data="data || []"
      :is-loading="isLoading"
      :css="datatableCss"
      not-found-msg="Items not found"
      @onUpdate="dtUpdateSort"
      trackBy="firstName"
    >
      <input
        slot="actions"
        slot-scope="props"
        type="button"
        class="btn btn-info"
        value="Edit"
        @click="dtEditClick(props);"
      >
      <input
        slot="actions"
        slot-scope="props"
        type="button"
        class="btn btn-info"
        value="Detalle"
        @click="dtEditClick(props);"
      >
      <div class="items-per-page" slot="ItemsPerPage">
        <label>Items per page</label>
        <ItemsPerPageDropdown
          :list-items-per-page="listItemsPerPage"
          :items-per-page="itemsPerPage"
          :css="itemsPerPageCss"
          @onUpdate="updateItemsPerPage"
        />
      </div>
      <Spinner slot="spinner"/>
    </DataTable>
  </div>
</template>
<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#app .title {
  margin-bottom: 30px;
}

#app .items-per-page {
  height: 100%;
  display: flex;
  align-items: center;
  color: #337ab7;
}

#app .items-per-page label {
  margin: 0 15px;
}

/* Datatable CSS */
#v-datatable-light .header-item {
  cursor: pointer;
  color: #337ab7;
  transition: color 0.15s ease-in-out;
}

#v-datatable-light .header-item:hover {
  color: #ed9b19;
}

#v-datatable-light .header-item.no-sortable {
  cursor: default;
}
#v-datatable-light .header-item.no-sortable:hover {
  color: #337ab7;
}

#v-datatable-light .header-item .th-wrapper {
  justify-content: center;
  display: flex;
  width: 95%;
  height: 95%;
  font-weight: bold;
}

#v-datatable-light .header-item .th-wrapper.checkboxes {
  justify-content: center;
}

#v-datatable-light .header-item .th-wrapper .arrows-wrapper {
  display: flex;
  flex-direction: column;
  margin-left: 10px;
  justify-content: space-between;
}

#v-datatable-light .header-item .th-wrapper .arrows-wrapper.centralized {
  justify-content: center;
}

#v-datatable-light .arrow {
  transition: color 0.15s ease-in-out;
  width: 0;
  height: 0;
  border-left: 8px solid transparent;
  border-right: 8px solid transparent;
}

#v-datatable-light .arrow.up {
  border-bottom: 8px solid #337ab7;
}

#v-datatable-light .arrow.up:hover {
  border-bottom: 8px solid #ed9b19;
}

#v-datatable-light .arrow.down {
  border-top: 8px solid #337ab7;
}

#v-datatable-light .arrow.down:hover {
  border-top: 8px solid #ed9b19;
}

#v-datatable-light .footer {
  display: flex;
  justify-content: space-between;
  width: 500px;
}
/* End Datatable CSS */

/* Pagination CSS */
#v-datatable-light-pagination {
  list-style: none;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  margin: 0;
  padding: 0;
  width: 300px;
  height: 30px;
}

#v-datatable-light-pagination .pagination-item {
  width: 30px;
  margin-right: 5px;
  font-size: 16px;
  transition: color 0.15s ease-in-out;
}

#v-datatable-light-pagination .pagination-item.selected {
  color: #ed9b19;
}

#v-datatable-light-pagination .pagination-item .page-btn {
  background-color: transparent;
  outline: none;
  border: none;
  color: #337ab7;
  transition: color 0.15s ease-in-out;
}

#v-datatable-light-pagination .pagination-item .page-btn:hover {
  color: #ed9b19;
}

#v-datatable-light-pagination .pagination-item .page-btn:disabled {
  cursor: not-allowed;
  box-shadow: none;
  opacity: 0.65;
}
/* END PAGINATION CSS */

/* ITEMS PER PAGE DROPDOWN CSS */
.item-per-page-dropdown {
  background-color: transparent;
  min-height: 30px;
  border: 1px solid #337ab7;
  border-radius: 5px;
  color: #337ab7;
}
.item-per-page-dropdown:hover {
  cursor: pointer;
}
/* END ITEMS PER PAGE DROPDOWN CSS */
</style>

<script>
import Spinner from "vue-simple-spinner";
import { DataTable, ItemsPerPageDropdown, Pagination } from "v-datatable-light";
import orderBy from "lodash.orderby";

const addZero = value => ("0" + value).slice(-2);

const formatDate = value => {
  if (value) {
    const dt = new Date(value);
    return `${addZero(dt.getDate())}/${addZero(
      dt.getMonth() + 1
    )}/${dt.getFullYear()}`;
  }
  return "";
};

const initialData = [
{
    ID: "19212132",
    Nombre: "Andrés López ",
    Ciudad: "Medellín",
    Adherencia: "Adherente",
    Estado:"activo",
    dob: "13/02/1975",
    created: new Date().getTime(),
    updated: new Date().getTime()
  },
  {
    ID: "15251621",
    Nombre: "Fernando Lara ",
    Ciudad: "Medellín",
    Adherencia: "No Adherente",
    Estado:"activo",
    dob: "16/04/1975",
    created: new Date().getTime(),
    updated: new Date().getTime()
  },
  {
    ID: "83261762",
    Nombre: "Edgar Noguera ",
    Ciudad: "Medellín",
    Adherencia: "No Adherente",
    Estado:"activo",
    dob: "13/02/1975",
    created: new Date().getTime(),
    updated: new Date().getTime()
  },
  {
    ID: "283474723",
    Nombre: "Victor Ramirez ",
    Ciudad: "Medellín",
    Adherencia: "Adherente",
    Estado:"activo",
    dob: "13/06/1987",
    created: new Date().getTime(),
    updated: new Date().getTime()
  },
  {
    ID: "845273526",
    Nombre: "Luis Hoyos ",
    Ciudad: "Medellín",
    Adherencia: "Adherente",
    Estado:"activo",
    dob: "23/10/1954",
    created: new Date().getTime(),
    updated: new Date().getTime()
  },
  {
    ID: "24523736",
    Nombre: "Carlos Alegría ",
    Ciudad: "Medellín",
    Adherencia: "Adherente",
    Estado:"activo",
    dob: "13/02/1965",
    created: new Date().getTime(),
    updated: new Date().getTime()
  },
  {
    ID: "12737261",
    Nombre: "Maria Obando ",
    Ciudad: "Medellín",
    Adherencia: "Adherente",
    Estado:"activo",
    dob: "13/05/1987",
    created: new Date().getTime(),
    updated: new Date().getTime()
  }
];

export default {
  name: "app",
  components: {
    DataTable,
    ItemsPerPageDropdown,
    Pagination,
    Spinner
  },
  data: function() {
    return {
      headerFields: [
        "__slot:checkboxes",
        {
          name: "ID",
          label: "ID",
          sortable: true
        },
        {
          name: "Nombre",
          label: "Nombre",
          sortable: true
        },
        {
          name: "Adherencia",
          label: "Adherencia",
          sortable: true
        },
          {
          name: "Estado",
          label: "Estado",
          sortable: true
        },
        {
          name: "dob",
          label: "F. Nac",
          sortable: true
        },
        {
          name: "created",
          label: "Creado",
          sortable: true,
          format: formatDate
        },
        {
          name: "updated",
          label: "Actualizado",
          sortable: false,
          format: formatDate
        },
        "__slot:actions"
      ],
      data: initialData.slice(0, 10),
      datatableCss: {
        table: "table table-bordered table-hover table-striped table-center",
        th: "header-item",
        thWrapper: "th-wrapper",
        thWrapperCheckboxes: "th-wrapper checkboxes",
        arrowsWrapper: "arrows-wrapper",
        arrowUp: "arrow up",
        arrowDown: "arrow down",
        footer: "footer"
      },
      paginationCss: {
        paginationItem: "pagination-item",
        moveFirstPage: "move-first-page",
        movePreviousPage: "move-previous-page",
        moveNextPage: "move-next-page",
        moveLastPage: "move-last-page",
        pageBtn: "page-btn"
      },
      itemsPerPageCss: {
        select: "item-per-page-dropdown"
      },
      isLoading: false,
      sort: "asc",
      sortField: "firstName",
      listItemsPerPage: [5, 10, 20, 50, 100],
      itemsPerPage: 10,
      currentPage: 1,
      totalItems: 16
    };
  },
  methods: {
    dtEditClick: props => alert("Click props:" + JSON.stringify(props)),

    dtUpdateSort: function({ sortField, sort }) {
      const sortedData = orderBy(initialData, [sortField], [sort]);
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = this.currentPage * this.itemsPerPage;
      this.data = sortedData.slice(start, end);
      console.log("load data based on new sort", this.currentPage);
    },

    updateItemsPerPage: function(itemsPerPage) {
      this.itemsPerPage = itemsPerPage;
      if (itemsPerPage >= initialData.length) {
        this.data = initialData;
      } else {
        this.data = initialData.slice(0, itemsPerPage);
      }
      console.log("load data with new items per page number", itemsPerPage);
    },

    changePage: function(currentPage) {
      this.currentPage = currentPage;
      const start = (currentPage - 1) * this.itemsPerPage;
      const end = currentPage * this.itemsPerPage;
      this.data = initialData.slice(start, end);
      console.log("load data for the new page", currentPage);
    },

    updateCurrentPage: function(currentPage) {
      this.currentPage = currentPage;
      console.log("update current page without need to load data", currentPage);
    }
  }
};
</script>
