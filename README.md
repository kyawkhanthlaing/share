# share

class PrintView(
    ctx: Context,
    private val viewModel: PrintReceiptViewModel,
    onBitmapCreated: (bitmap: Bitmap) -> Unit,
)

    : LinearLayoutCompat(ctx) {

    var status = mutableStateOf("")


    init {
        val width = 400
        val height = 250

        val view = ComposeView(ctx)
        view.visibility = View.GONE
        view.layoutParams = LayoutParams(width, height)
        this.addView(view)

        view.setContent {
            when (status.value) {
                "p1" -> {
                    PrintReceipt1(viewModel = viewModel)
                }
                "p2" -> {
                    PrintReceipt2(viewModel = viewModel)
                }
                else -> {
                    PrintReceipt3(viewModel = viewModel)
                }
            }

        }

        viewTreeObserver.addOnGlobalLayoutListener(object :
            ViewTreeObserver.OnGlobalLayoutListener {
            override fun onGlobalLayout() {
                val graphicUtils = GraphicUtils()
                val bitmap =
                    graphicUtils.createBitmapFromView(view = view, width = width, height = height)
                onBitmapCreated(bitmap)
                viewTreeObserver.removeOnGlobalLayoutListener(this)

                Timber.tag("createdBitmap").d(bitmap.toString())
            }

        })

    }
}



-------------------------------


    AndroidView(

        factory = { context ->

           
            TextView(context).apply {
                text="Hello"
            }

        },
        update = {
          
            it.text="Print"
        }

    )
    AndroidView(
        factory = { context ->
            val printView2 =
                PrintView(ctx = context, viewModel = viewModel) { bitmap ->
                    onBitmapCreated2(bitmap)
                }
            printView2

        },
        update = {

        }
    )
