```



import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.graphics.drawable.BitmapDrawable;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.view.View;




public class SideBar extends View {

	private static final int MARGIN_BOTTOM = 2;

	private int mItemHeight = 18;
	private OnLetterTouchListener mOnLetterTouchListener;
	private Paint mPaint;
	private float mWidthCenter;
	private int backgroundColor = 0x00F0F0F0;
	
	private char[] chars = AppCommon.INDEXER_CHARS;

	public interface OnLetterTouchListener {

		public abstract void onPressedDown(int index, char ch);

		public abstract void onPressedUp(int index, char ch);
	}

	public SideBar(Context context) {
		this(context, null);
	}

	public SideBar(Context context, AttributeSet attrs) {
		this(context, attrs, -1);
	}

	public SideBar(Context context, AttributeSet attrs, int defStyle) {
		super(context, attrs, defStyle);
		init();
	}

	private int startY = 0;//其实位置的y轴坐标
	public SideBar setChars(char[] chars) {
		this.chars = chars;
//		for(int i = 0 ; i < (26 - chars.length) / 2 ; i++){
//			startY += i * mItemHeight;
//		}

		init();
		postInvalidate();
		return this;
	}

	private void init() {
		mPaint = new Paint();
		mPaint.setColor(0xFFCFCFD0);
		mPaint.setAntiAlias(true);
		mPaint.setFakeBoldText(true);
		mPaint.setTextSize(32);
		mPaint.setColor(getResources().getColor(R.color.blue));
		mPaint.setSubpixelText(true);
		mPaint.setTextAlign(Paint.Align.CENTER);
		setBackgroundColor(backgroundColor);
	}

	public boolean onTouchEvent(MotionEvent event) {
		super.onTouchEvent(event);
		int idx = (int) event.getY() / mItemHeight;
		if (idx >= chars.length) {
			idx = chars.length - 1;
		} else if (idx < 0) {
			idx = 0;
		}
		if (event.getAction() == MotionEvent.ACTION_DOWN || event.getAction() == MotionEvent.ACTION_MOVE) {
			setBackgroundResource(R.color.sidebar_select_background);
			if (mOnLetterTouchListener != null) {
				mOnLetterTouchListener.onPressedDown(idx, chars[idx]);
			}
		} else if (event.getAction() == MotionEvent.ACTION_UP) {
			setBackgroundDrawable(new BitmapDrawable());
			setBackgroundColor(backgroundColor);
			if (mOnLetterTouchListener != null) {
				mOnLetterTouchListener.onPressedUp(idx, chars[idx]);
			}
		}
		return true;
	}


	@Override
	protected void onSizeChanged(int w, int h, int oldw, int oldh) {
		mItemHeight = (h - MARGIN_BOTTOM) / 26;
		mWidthCenter = getMeasuredWidth() / 2;
		super.onSizeChanged(w, h, oldw, oldh);
	}

	protected void onDraw(Canvas canvas) {
		for (int i = 0; i < chars.length; i++) {
			canvas.drawText(String.valueOf(chars[i]), mWidthCenter,startY + mItemHeight + (i * mItemHeight), mPaint);
		}
		super.onDraw(canvas);
	}

	public void setOnLetterTouchListener(OnLetterTouchListener listener) {
		mOnLetterTouchListener = listener;
	}
}

```