---
title: ListView优化
date: 2017-01-22 21:46:57
tags: 
- Android
- ListView
categories: 移动开发
---

```java

@Override
    public View getView(int position, View convertView, ViewGroup parent) {
        ViewHolder viewHolder;
        if (null == convertView) {
            convertView = LayoutInflater.from(context).inflate(R.layout.item_category, parent, false);
            viewHolder = new ViewHolder();
            viewHolder.title = (ScaleTextView) convertView.findViewById(R.id.text);
            convertView.setTag(viewHolder);
        } else {
            viewHolder = (ViewHolder) convertView.getTag();
        }
        viewHolder.title.setText(getItem(position).getText());

        return convertView;
    }

    private static class ViewHolder {
        TextView title;
    }
```