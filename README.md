Android-DraggableGridViewPager
==============================

Zaker style grid view pager, support dragging & rearrange, using as zaker's main screen.

Features
========

+ Android SDK **level 4+**.
+ Grid view layout split into pages.
+ Horizontally **swipe** pages like ViewPager in support-v4 library.
+ Long press to **dragging & rearrange** grid item.
+ Setting col & row count.
+ Setting listeners for: page change, item click, item long click, rearrange.

Snapshots
=========

[idle]: https://github.com/zzhouj/Android-DraggableGridViewPager/raw/master/snapshot/idle.png  "idle"
[swipe]: https://github.com/zzhouj/Android-DraggableGridViewPager/raw/master/snapshot/swipe.png  "swipe"
[dragging]: https://github.com/zzhouj/Android-DraggableGridViewPager/raw/master/snapshot/dragging.png  "dragging"

![idle snapshot][idle]
![swipe snapshot][swipe]
![dragging snapshot][dragging]

Usage
=====
1. Add layout xml fragment like below:

		<com.coco.draggablegridviewpager.DraggableGridViewPager
		    android:id="@+id/draggable_grid_view_pager"
		    android:layout_width="match_parent"
		    android:layout_height="match_parent"
		    android:paddingBottom="20dp"
		    android:paddingLeft="20dp"
		    android:paddingRight="60dp"
		    android:paddingTop="20dp" >
		
		</com.coco.draggablegridviewpager.DraggableGridViewPager>
		
2. Define Adapter to provide item data & view:

		private ArrayAdapter<String> mAdapter;
		
		// ...
		
		mAdapter = new ArrayAdapter<String>(this, 0) {
			@Override
			public View getView(int position, View convertView, ViewGroup parent) {
				final String text = getItem(position);
				if (convertView == null) {
					convertView = (TextView) getLayoutInflater().inflate(R.layout.draggable_grid_item, null);
				}
				((TextView) convertView).setText(text);
				return convertView;
			}
		
		};
		

3. Then find the **DraggableGridViewPager** view install listeners to it:

		@Override
		protected void onCreate(Bundle savedInstanceState) {
			requestWindowFeature(Window.FEATURE_NO_TITLE);
			super.onCreate(savedInstanceState);
		
			setContentView(R.layout.draggable_grid_view_pager_test);
			mDraggableGridViewPager = (DraggableGridViewPager) findViewById(R.id.draggable_grid_view_pager);
		
			// ...
		
			mDraggableGridViewPager.setAdapter(mAdapter);
			mDraggableGridViewPager.setOnPageChangeListener(new OnPageChangeListener() {
				@Override
				public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {
					Log.v(TAG, "onPageScrolled position=" + position + ", positionOffset=" + positionOffset
							+ ", positionOffsetPixels=" + positionOffsetPixels);
				}
		
				@Override
				public void onPageSelected(int position) {
					Log.i(TAG, "onPageSelected position=" + position);
				}
		
				@Override
				public void onPageScrollStateChanged(int state) {
					Log.d(TAG, "onPageScrollStateChanged state=" + state);
				}
			});
			mDraggableGridViewPager.setOnItemClickListener(new OnItemClickListener() {
				@Override
				public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
					showToast(((TextView) view).getText().toString());
				}
			});
			mDraggableGridViewPager.setOnItemLongClickListener(new OnItemLongClickListener() {
				@Override
				public boolean onItemLongClick(AdapterView<?> parent, View view, int position, long id) {
					showToast(((TextView) view).getText().toString() + " long clicked!!!");
					return true;
				}
			});
			mDraggableGridViewPager.setOnRearrangeListener(new OnRearrangeListener() {
				@Override
				public void onRearrange(int oldIndex, int newIndex) {
					Log.i(TAG, "OnRearrangeListener.onRearrange from=" + oldIndex + ", to=" + newIndex);
					String item = mAdapter.getItem(oldIndex);
					mAdapter.setNotifyOnChange(false);
					mAdapter.remove(item);
					mAdapter.insert(item, newIndex);
					mAdapter.notifyDataSetChanged();
				}
			});
		
			// ...
		
		}


License
=======

	The MIT License (MIT)
	
	Copyright (c) 2014 justin
	
	Permission is hereby granted, free of charge, to any person obtaining a copy of
	this software and associated documentation files (the "Software"), to deal in
	the Software without restriction, including without limitation the rights to
	use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
	the Software, and to permit persons to whom the Software is furnished to do so,
	subject to the following conditions:
	
	The above copyright notice and this permission notice shall be included in all
	copies or substantial portions of the Software.
	
	THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
	IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
	FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
	COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
	IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
	CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
