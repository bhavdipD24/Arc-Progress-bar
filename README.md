# Arc-Progress-bar
This is just a basic example of how to create an arc progress bar using Jetpack Compose with Canvas API.  You can customize the progress bar by changing the colors, sizes, and other parameters. You can also add animations to the progress bar to make it more visually appealing.

<pre><code>
@Composable
fun DrawArcAnimation(modifier: Modifier) {
    var progress by remember {
        mutableStateOf(0f)
    }

    val progressAnimation = animateFloatAsState(
        targetValue = progress,
        animationSpec = spring(
            dampingRatio = Spring.DampingRatioMediumBouncy,
            stiffness = Spring.StiffnessLow
        ), label = ""
    )

    var offset by remember {
        mutableStateOf(Offset(x = 0f, y = 0f))
    }

    Box(modifier = modifier, contentAlignment = Alignment.Center)
    {

        val boxModifier = Modifier.size(180.dp)

        val startAngel = -180f
        val sweepAngel = 180f
        val strokeWidth = 20.dp


        Box(
            modifier = boxModifier,
        ) {


            Canvas(modifier = boxModifier.padding(10.dp)) {
                val width = size.width
                val height = size.height
                val stroke = Stroke(strokeWidth.toPx(), cap = StrokeCap.Round)
                
                val percentageProcess = sweepAngel * progressAnimation.value
                val angleInDegrees = percentageProcess.toDouble() + 90f
                val radius = (height / 2)
                val x =
                    -(radius * kotlin.math.sin(Math.toRadians(angleInDegrees))).toFloat() + (width / 1.95f)
                val y =
                    (radius * kotlin.math.cos(Math.toRadians(angleInDegrees))).toFloat() + (height / 1.95f)

                offset = Offset(x, y)

                val grendient = Brush.linearGradient(
                    listOf(
                        Color("#2E3192".toColorInt()),
                        Color("#1BFFFF".toColorInt()),
                        Color("#1BFFFF".toColorInt()),
                    )
                )

                drawArc(
                    color = Color.LightGray.copy(alpha = 0.2f),
                    startAngle = startAngel,
                    sweepAngle = sweepAngel,
                    useCenter = false,
                    size = Size(width = width, height = height),
                    style = stroke
                )


                drawArc(
                    brush = grendient,
                    startAngle = startAngel,
                    sweepAngle = percentageProcess,
                    useCenter = false,
                    size = Size(width = width, height = height),
                    style = stroke
                )

            }

            Box(
                modifier = boxModifier
                    .offset { IntOffset(offset.x.roundToInt(), offset.y.roundToInt()) }
            ) {
                Box(
                    modifier = Modifier
                        .size(strokeWidth * 0.8f)
                        .background(color = Color("#93A5CF".toColorInt()), shape = CircleShape)
                )
            }
        }


        Text(
            text = "" + (progressAnimation.value * 100).roundToInt()+"%",
            modifier = Modifier
                .clip(RoundedCornerShape(20.dp))
                .clickable {
                    progress = if (progress == 1f) {
                        0.2f
                    } else {
                        Random.nextDouble(0.1,1.0).toFloat()
                    }
                }
                .padding(10.dp), fontSize = 18.sp)

    }
}

</code></pre>
