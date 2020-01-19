```

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.util.AttributeSet;
import android.widget.TextView;

/**
 * Created by wangliang on  0016/2016/12/16.
 * 创建时间：  0016/2016/12/16.
 * 创建人：王亮（Loren wang）
 * 功能作用：文字总是在底部的textview
 * 思路：
 * 修改人：
 * 修改时间：
 * 备注：
 */
public class NoPaddingTextView extends TextView {
    private String TAG = getClass().getName();
    private Paint paint;

    public NoPaddingTextView(Context context) {
        super(context);
    }

    public NoPaddingTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public NoPaddingTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec,heightMeasureSpec);
        paint = new Paint();
        paint.setTextSize(getTextSize());
        paint.setAntiAlias(true);
        paint.setColor(getTextColors().getDefaultColor());

        Paint.FontMetrics fontMetrics = paint.getFontMetrics();

        setMeasuredDimension((int) paint.measureText((String) getText()),
                (int) (fontMetrics.descent - fontMetrics.ascent) + 10);

    }

    @Override
    protected void onDraw(Canvas canvas) {
        LogUtils.logD(TAG,getText().toString());
        String text = (String) getText();
        canvas.drawText(text
                ,0,getHeight() - 5,paint);
    }


    @Override
    public void setText(CharSequence text, BufferType type) {
        postInvalidate();
        super.setText(text, type);
    }
}

```