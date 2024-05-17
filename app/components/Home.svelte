<page>
      <actionBar title="My Tasks" />
      <tabView>
        <tabViewItem title="To Do">
           <gridLayout columns="*,120" rows="70,*">
            <textField col="0" row="0" bind:text="{textFieldValue}" hint="Type new task" editable="true"
            on:returnPress="{onButtonTap}"/>
            <button col="1" row="0" text="Add task" on:tap="{onButtonTap}" class="-primary"/>
            <listView items="{todos}" on:itemTap="{onItemTap}" row="1" colSpan="2">
                <Template let:item>
                    <label text="{item.name}" textWrap="true" />
                </Template>
            </listView>
           </gridLayout>
        </tabViewItem>
        <tabViewItem title="Completed">
            <listView items="{dones}" on:itemTap="{onDonetap}">
            <Template let:item>
                <label text="{item.name}" textWrap="true" class="todo-item-completed" />
            </Template>
            </listView>
        </tabViewItem>
      </tabView>
</page>
    <script>

        import { Template } from 'svelte-native/components';

        let todos = [];
        let textFieldValue="";
        let dones = [];
        const removeFormList = (list, item) => list.filter(t => t !== item);
        const addToList = (list, item) => [item, ...list];

        async function onItemTap(args){
            console.log(`Item ${todos[args.index].name} at index: ${args.index} was tapped`);
            let result = await action("What do you want do do with this task?", "Cancel", ['Mark completed', 'Delete forever']);

            console.log(result);

            let item = todos[args.index];
            switch(result){
                case "Mark completed":
                    dones = addToList(dones, item);
                    todos = removeFormList(todos, item);
                    break;
                case "Delete forever":
                    todos = removeFormList(todos, item);
                    break;
                case "Cancel" || undefined:
                    break;
            }
        }

        function onButtonTap(){
            if(textFieldValue ==="") return;
            console.log('New task added: ' + textFieldValue + ".");
            todos = [{name: textFieldValue}, ...todos];
            textFieldValue = "";
        }

        async function onDonetap(args){
            let result = await action("What do you want do do with this task?", "Cancel", ['Mark To Do', 'Delete forever']);

            console.log(result);
            let item = dones[args.index];

            switch(result){
                case "Mark To Do":
                    todos = addToList(todos, item);
                    dones = removeFormList(dones, item);
                    break;
                case "Delete forever":
                    dones = removeFormList(dones, item);
                    break;
                case "Cancel" || undefined:
                    break;
            }
        }
    </script>
    <style>
        textField {
                font-size: 20;
        }
        .todo-item-completed {
            color: #939393;
            text-decoration: line-through;
        }
        label {
            font-size: 18;
        }
    </style>

