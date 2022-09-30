- 👋 Hi, I’m @wingfrredom1
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
wingfrredom1/wingfrredom1 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
Wings of frredom
import com.termux.shared.termux.TermuxConstants.TERMUX_APP.TERMUX_SERVICE;

...

public class PluginResultsService extends IntentService {

    public static final String EXTRA_EXECUTION_ID = "execution_id";

    private static int EXECUTION_ID = 1000;

    public static final String PLUGIN_SERVICE_LABEL = "PluginResultsService";

    private static final String LOG_TAG = "PluginResultsService";

    public PluginResultsService(){
        super(PLUGIN_SERVICE_LABEL);
    }

    @Override
    protected void onHandleIntent(@Nullable Intent intent) {
        if (intent == null) return;

        Log.d(LOG_TAG, PLUGIN_SERVICE_LABEL + " received execution result");

        final Bundle resultBundle = intent.getBundleExtra(TERMUX_SERVICE.EXTRA_PLUGIN_RESULT_BUNDLE);
        if (resultBundle == null) {
            Log.e(LOG_TAG, "The intent does not contain the result bundle at the \"" + TERMUX_SERVICE.EXTRA_PLUGIN_RESULT_BUNDLE + "\" key.");
            return;
        }

        final int executionId = intent.getIntExtra(EXTRA_EXECUTION_ID, 0);

        Log.d(LOG_TAG, "Execution id " + executionId + " result:\n" +
                "stdout:\n```\n" + resultBundle.getString(TERMUX_SERVICE.EXTRA_PLUGIN_RESULT_BUNDLE_STDOUT, "") + "\n```\n" +
                "stdout_original_length: `" + resultBundle.getString(TERMUX_SERVICE.EXTRA_PLUGIN_RESULT_BUNDLE_STDOUT_ORIGINAL_LENGTH) + "`\n" +
                "stderr:\n```\n" + resultBundle.getString(TERMUX_SERVICE.EXTRA_PLUGIN_RESULT_BUNDLE_STDERR, "") + "\n```\n" +
                "stderr_original_length: `" + resultBundle.getString(TERMUX_SERVICE.EXTRA_PLUGIN_RESULT_BUNDLE_STDERR_ORIGINAL_LENGTH) + "`\n" +
                "exitCode: `" + resultBundle.getInt(TERMUX_SERVICE.EXTRA_PLUGIN_RESULT_BUNDLE_EXIT_CODE) + "`\n" +
                "errCode: `" + resultBundle.getInt(TERMUX_SERVICE.EXTRA_PLUGIN_RESULT_BUNDLE_ERR) + "`\n" +
                "errmsg: `" + resultBundle.getString(TERMUX_SERVICE.EXTRA_PLUGIN_RESULT_BUNDLE_ERRMSG, "") + "`");
    }

    public static synchronized int getNextExecutionId() {
        return EXECUTION_ID++;
    }

}
