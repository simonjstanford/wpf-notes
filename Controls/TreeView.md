# TreeView

You can bind to a DataRelation object when working with DataTables to display hierarchical data:

```csharp
SqlConnection con = new SqlConnection(connectionString);
SqlCommand cmd = new SqlCommand("GetProducts", con);
cmd.CommandType = CommandType.StoredProcedure;
SqlDataAdapter adapter = new SqlDataAdapter(cmd);
DataSet ds = new DataSet();
adapter.Fill(ds, "Products");
cmd.CommandText = "GetCategories";
adapter.Fill(ds, "Categories");
// Set up a relation between these tables.
DataRelation relCategoryProduct = new DataRelation("CategoryProduct",
   ds.Tables["Categories"].Columns["CategoryID"],
   ds.Tables["Products"].Columns["CategoryID"]);
ds.Relations.Add(relCategoryProduct);
return ds;

//then bind to:
treeCategories.ItemsSource = ds.Tables["Categories"].DefaultView;
```

```xml
<TreeView Name="treeCategories" Margin="5">
   <TreeView.ItemTemplate>
      <HierarchicalDataTemplate ItemsSource="{Binding CategoryProduct}">
         <TextBlock Text="{Binding CategoryName}" Padding="2" />
         <HierarchicalDataTemplate.ItemTemplate>
            <DataTemplate>
               <TextBlock Text="{Binding ModelName}" Padding="2" />
            </DataTemplate>
         </HierarchicalDataTemplate.ItemTemplate>
      </HierarchicalDataTemplate>
   </TreeView.ItemTemplate>
</TreeView>
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbNTUyNjExNjNdfQ==
-->