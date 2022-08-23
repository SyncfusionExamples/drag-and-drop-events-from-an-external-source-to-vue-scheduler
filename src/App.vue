<template>
<div>
  <div class='title-container'>
    <div class="scheduler-title">Doctor's Appointments</div>
    <div class="treeview-title">Waiting List</div>
  </div>
  <div class='component-container'>
    <div class='scheduler-component'>
      <ejs-schedule height="550px" :selectedDate='schedulerSelectedDate' 
      :eventSettings="appointmentData" ref="schedulerObject">
      </ejs-schedule>
    </div>
    <div class='treeview-component'>
      <ejs-treeview :fields='treeviewFields' :allowDragAndDrop='true'
      :nodeDragStop="onTreeDragStop" ref="treeviewObject">
      </ejs-treeview>
    </div>
  </div>
</div>

</template>

<script> 
import { ScheduleComponent, Day, Week, WorkWeek, Month, Agenda } from "@syncfusion/ej2-vue-schedule";
 import { TreeViewComponent } from '@syncfusion/ej2-vue-navigations';

export default {
  name: 'App',
  components: {
    'ejs-schedule': ScheduleComponent,
    'ejs-treeview' : TreeViewComponent
  },
  provide : {
    schedule : [Day, Week, WorkWeek, Month, Agenda]
  },
  data() {
    return {
      draggedItemId : null,   
      treeviewFields: { 
        dataSource: [
          {Id: 1, Name: 'Peter'},
          {Id: 2, Name: 'James'},
          {Id: 3, Name: 'David'},
          {Id: 4, Name: 'John'},
          {Id: 5, Name: 'Steve'}
        ],
        id:'Id', text:'Name'
      },
      schedulerSelectedDate : new Date(2022, 5, 9),
      appointmentData : {
         dataSource : [
             {
              Id : 1,
              Subject : 'John',
              StartTime: new Date(2022, 5, 6, 8, 0, 0),
              EndTime: new Date(2022, 5, 6, 9, 0, 0)
            },
            {
              Id : 2,
              Subject : 'Steve',
              StartTime: new Date(2022, 5, 8, 10, 0, 0),
              EndTime: new Date(2022, 5, 8, 11, 30, 0)
            }
         ]
      }
    };
  },
  methods : {
    onTreeDragStop : function(args) {
      args.cancel = true;
      let schedulerComponentObject = this.$refs.schedulerObject.ej2Instances;
      let cellData = schedulerComponentObject.getCellDetails(args.target);
      let treeviewComponentObject = this.$refs.treeviewObject.ej2Instances;
      let filteredData = treeviewComponentObject.fields.dataSource.filter(function (item) { return item.Id === parseInt(args.draggedNodeData.id); });
      let eventData = {
        Subject : filteredData[0].Name,
        StartTime : cellData.startTime,
        EndTime : cellData.endTime,
        IsAllDay : cellData.isAllDay
      };
      //schedulerComponentObject.addEvent(eventData);
      schedulerComponentObject.openEditor(eventData,'Add',true);

    }
  }
}
</script>

<style>
  @import "../node_modules/@syncfusion/ej2-base/styles/material.css";
  @import "../node_modules/@syncfusion/ej2-vue-navigations/styles/material.css";
  @import "../node_modules/@syncfusion/ej2-inputs/styles/material.css";
  @import "../node_modules/@syncfusion/ej2-buttons/styles/material.css";

  .title-container {
        display: inline-block;
        text-align: center;
        font-weight: bold;
        width: 100%;   
        padding-top: 10px;
        padding-bottom: 10px;   
    }
    .scheduler-title {
        width: 80%;
        float: left;
    }
    .treeview-title {
        width: 20%;
        float: right;
    }
    .component-container {
        display: inline-block;
        width: 100%;   
    }
    .scheduler-component {
        padding-left: 10px;
        float: left;
        width: 80%;
    }
    .treeview-component {
        float: right;
        width: 15%;
    }

  @import '../node_modules/@syncfusion/ej2-base/styles/material.css';
  @import '../node_modules/@syncfusion/ej2-buttons/styles/material.css';
  @import '../node_modules/@syncfusion/ej2-calendars/styles/material.css';
  @import '../node_modules/@syncfusion/ej2-dropdowns/styles/material.css';
  @import '../node_modules/@syncfusion/ej2-inputs/styles/material.css';
  @import '../node_modules/@syncfusion/ej2-navigations/styles/material.css';
  @import '../node_modules/@syncfusion/ej2-popups/styles/material.css';
  @import '../node_modules/@syncfusion/ej2-vue-schedule/styles/material.css';
</style>
