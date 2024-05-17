<script>
    const { tabView, tabViewItem }=require("@nativescript/core");
</script>
    <page>
      <actionBar title="My Tasks" />
      <tabView>
        <tabViewItem title="To Do">
            <label textWrap="true">Active tasks</label>
        </tabViewItem>
        <tabViewItem title="Completed">
            <label textWrap="true">Completed tasks</label>
        </tabViewItem>
      </tabView>
    </page>

