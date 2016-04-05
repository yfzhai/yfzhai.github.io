---
layout: posts
title: "ExpandableListView折叠之前展开的group"
---

## ExpandableListView折叠之前展开的group
----------------------------------
下面代码可实现当点击其中一个分组时，折叠另一个已展开的分组。    
<font size=4px>
<xmp class="prettyprint linenums">
final ExpandableListView  eListView = getExpandableListView();
getExpandableListView().setOnGroupClickListener(new ExpandableListView.OnGroupClickListener() {
	@Override
	public boolean onGroupClick(ExpandableListView parent,
			View source, int groupPosition, long id) {
		if(expandFlag == -1){
			//展开被选中的组
			eListView.expandGroup(groupPosition);
			//设置被选中的组位于顶端(新版本Android已不可用)
			eListView.setSelectedGroup(groupPosition);
			expandFlag = groupPosition;
		}else if(expandFlag == groupPosition){
			eListView.collapseGroup(expandFlag);
			expandFlag = -1;
		}else{
			eListView.collapseGroup(expandFlag);
			eListView.expandGroup(groupPosition);
			eListView.setSelectedGroup(groupPosition);
			expandFlag = groupPosition;
		}
		return true;
	}
});
</xmp>
</font>      