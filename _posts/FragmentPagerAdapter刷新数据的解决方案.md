---
title: FragmentPagerAdapter刷新数据的解决方案
date: 2017-01-12 20:29:06
tags: 
- Android
- Fragment
categories: 移动开发
---

# ViewPager+FragmentPagerAdapter+TabLayout

上代码
```java
	private class MyFragmentPagerAdapter extends FragmentPagerAdapter {
        final int PAGE_COUNT = 3;
        private String mTabTitles[];

        MyFragmentPagerAdapter(FragmentManager fm, String[] tabTitles) {
            super(fm);
            mTabTitles = tabTitles;
        }

        @Override
        public Fragment getItem(int position) {
            switch (position) {
                case 0:
                    return AFragment.newInstance();
                case 1:
                    return BFragment.newInstance();
                case 2:
                    return CFragment.newInstance();
                default:
                    return DFragment.newInstance();
            }
        }

        @Override
        public Object instantiateItem(ViewGroup container, int position) {
            return super.instantiateItem(container, position);
        }

        //在Frament中对外提供一个方法用于刷新数据
        @Override
        public int getItemPosition(Object object) {
            if (object instanceof AFragment) {
                ((AFragment) object).refreshData();
            } else if (object instanceof BFragment) {
                ((BFragment) object).refreshData();
            } else if (object instanceof CFragment) {
                ((CFragment) object).refreshData();
            }
            return super.getItemPosition(object);
        }

        @Override
        public int getCount() {
            return PAGE_COUNT;
        }

        @Override
        public CharSequence getPageTitle(int position) {
            return mTabTitles[position];
        }
    }

```