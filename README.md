# All-Do-list
Contractor services app
// Android

// MainActivity.java

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Initialize Firebase
        FirebaseApp.initializeApp(this);

        // Set up the RecyclerView
        RecyclerView recyclerView = findViewById(R.id.recycler_view);
        recyclerView.setLayoutManager(new LinearLayoutManager(this));
        recyclerView.setHasFixedSize(true);

        // Set up the Firebase Database
        FirebaseDatabase database = FirebaseDatabase.getInstance();
        DatabaseReference databaseReference = database.getReference("services");

        // Set up the FirebaseUI Adapter
        FirebaseRecyclerOptions<Service> options =
                new FirebaseRecyclerOptions.Builder<Service>()
                        .setQuery(databaseReference, Service.class)
                        .build();

        FirebaseRecyclerAdapter<Service, ServiceViewHolder> adapter =
                new FirebaseRecyclerAdapter<Service, ServiceViewHolder>(options) {
                    @Override
                    protected void onBindViewHolder(@NonNull ServiceViewHolder holder, int position, @NonNull Service model) {
                        holder.bind(model);
                    }

                    @NonNull
                    @Override
                    public ServiceViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
                        View view = LayoutInflater.from(parent.getContext())
                                .inflate(R.layout.service_item, parent, false);

                        return new ServiceViewHolder(view);
                    }
                };

        recyclerView.setAdapter(adapter);
    }

    @Override
    protected void onStart() {
        super.onStart();
    }

    @Override
    protected void onStop() {
        super.onStop();
    }
}

// ServiceViewHolder.java

public class ServiceViewHolder extends RecyclerView.ViewHolder {

    private TextView serviceNameTextView;
    private TextView serviceDescriptionTextView;
    private ImageView serviceImageView;

    public ServiceViewHolder(@NonNull View itemView) {
        super(itemView);

        serviceNameTextView = itemView.findViewById(R.id.service_name_text_view);
        serviceDescriptionTextView = itemView.findViewById(R.id.service_description_text_view);
        serviceImageView = itemView.findViewById(R.id.service_image_view);
    }

    public void bind(Service service) {
        serviceNameTextView.setText(service.getName());
        serviceDescriptionTextView.setText(service.getDescription());
        serviceImageView.setImageResource(service.getImage());
    }
}

// iOS

// AppDelegate.swift

import UIKit
import Firebase

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        FirebaseApp.configure()
        return true
    }

// ServiceTableViewCell.swift

import UIKit

class ServiceTableViewCell: UITableViewCell {

    @IBOutlet weak var serviceNameLabel: UILabel!
    @IBOutlet weak var serviceDescriptionLabel: UILabel!
    @IBOutlet weak var serviceImageView: UIImageView!

    func configure(with service: Service) {
        serviceNameLabel.text = service.name
        serviceDescriptionLabel.text = service.description
        serviceImageView.image = UIImage(named: service.image)
    }

}

// ViewController.swift

import UIKit
import Firebase

class ViewController: UIViewController {

    @IBOutlet weak var tableView: UITableView!

    var services = [Service]()

    override
