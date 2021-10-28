---
description: 数据模块包括DataView体系模块，数据更新模块Das。
---

# 数据处理DataView

## 加载数据、更新、执行SQL的操作

例子的功能是从前端返回一个数据实体，通过id相同就更新到数据库，不存在的id就删除

加载数据

```java
IDataView targetDV = das.find("select * from " + targetName + " where id in (:ids)", params);
```

遍历数据

```java
Map<String, IDataObject> modifiedMap = new HashMap<String, IDataObject>();
	List<IDataObject> updateList = targetDV.select();
	for(IDataObject o : updateList) {
		modifiedMap.put(o.getId(), o);
	}
```

按id更新数据，数据只更新到内存，保存数据需要另外方法。

```java
for (IDataObject obj : records) {
		IDataObject object = modifiedMap.get(obj.getId());
		if(object == null) {
			object = targetDV.insertDataObject();
		}
		
		for(String fd : column) {
			if(!fd.startsWith("H_")) {
				Object val = obj.get(fd);
				object.set(fd, val);
			}
		}
	}
```

保存数据到数据库

```java
targetDV.acceptChanges();
```

删除多余的数据

```java
		//删除数据
		das.executeUpdate("delete from " + targetName + " where id in (:ids)", params);
```

完整代码

```java

public class UpdateRecord implements IRule {

	private static final Logger log = LoggerFactory.getLogger(UpdateRecord.class);
	public static final String D_CODE="updateRecordYindangu";
	public static final String D_InputEntity="inputEntity";
	public static final String D_SaveAll="isSaveAll",D_OutUpdateCount="updateCount",D_TableName="tableName";

	@Override
	public IRuleOutputVo evaluate(IRuleContext context) {
		try {
			Object dv =context.getVObject().getInput(D_InputEntity);
			IDataView dataView = (IDataView)dv;// (DataView) getDataViewWithType(context, sourceName, dataSourceType);
			String tableName =(String) context.getInput(D_TableName);
			List<IDataObject> records = dataView.select();
			int updateCount = updateLogic(tableName, records);
			return context.newOutputVo().put(D_OutUpdateCount, updateCount).setSuccess(true);
		}
		catch(SQLException e) {
			log.error("",e);
			return context.newOutputVo().setSuccess(false).setMessage(e.getMessage());
		}
	}
 
 
	/**
	 * 把前端的数据按id保存到数据库
	 * @param targetName
	 * @param targetNameMapping
	 * @param addList
	 */
	@SuppressWarnings("rawtypes")
	private int updateLogic(String targetName,   List<IDataObject> records) throws SQLException{
		if (VdsUtils.collection.isEmpty(records)) {
			return 0;
		} 
		VDS vds = VDS.getIntance();
		List<String> idParams = new ArrayList<String>(); 
		for (IDataObject obj : records) {
			// obj.getId()如果为空，不需要处理，使用UUID产生的数据在数据库中不可能存在
			String newId = obj.getId();
			if (!VdsUtils.string.isEmpty(newId)) {
				idParams.add(newId); 
			}
		}
		Map params = Collections.singletonMap("ids", idParams);
		IDAS das = vds.getDas();
		IDataView targetDV = das.find("select * from " + targetName + " where id in (:ids)", params);
		Set<String> column = targetDV.getMetadata().getColumnNames(); 
		
		Map<String, IDataObject> modifiedMap = new HashMap<String, IDataObject>();
		List<IDataObject> updateList = targetDV.select();
		for(IDataObject o : updateList) {
			modifiedMap.put(o.getId(), o);
		}
		for (IDataObject obj : records) {
			IDataObject object = modifiedMap.get(obj.getId());
			if(object == null) {
				object = targetDV.insertDataObject();
			}
			
			for(String fd : column) {
				if(!fd.startsWith("H_")) {
					Object val = obj.get(fd);
					object.set(fd, val);
				}
			}
		}
 
		targetDV.acceptChanges();
		
		//删除数据
		das.executeUpdate("delete from " + targetName + " where id in (:ids)", params);
		return records.size();
	}
}
```
