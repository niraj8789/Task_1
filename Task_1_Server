import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoCursor;
import com.mongodb.client.MongoDatabase;
import org.bson.Document;

import java.util.ArrayList;
import java.util.List;

public class ServerDao {
    private final MongoCollection<Document> collection;

    public ServerDao() {
        MongoDatabase database = MongoClients.create().getDatabase("mydb");
        this.collection = database.getCollection("servers");
    }

    public List<Server> findAll() {
        List<Server> servers = new ArrayList<>();
        MongoCursor<Document> cursor = collection.find().iterator();
        try {
            while (cursor.hasNext()) {
                Document doc = cursor.next();
                servers.add(new Server(
                        doc.getString("id"),
                        doc.getString("name"),
                        doc.getString("language"),
                        doc.getString("framework")
                ));
            }
        } finally {
            cursor.close();
        }
        return servers;
    }

    public Server findById(String id) {
        Document doc = collection.find(new Document("id", id)).first();
        if (doc == null) {
            return null;
        }
        return new Server(
                doc.getString("id"),
                doc.getString("name"),
                doc.getString("language"),
                doc.getString("framework")
        );
    }

    public List<Server> findByName(String name) {
        List<Server> servers = new ArrayList<>();
        MongoCursor<Document> cursor = collection.find(new Document("name", new Document("$regex", name))).iterator();
        try {
            while (cursor.hasNext()) {
                Document doc = cursor.next();
                servers.add(new Server(
                        doc.getString("id"),
                        doc.getString("name"),
                        doc.getString("language"),
                        doc.getString("framework")
                ));
            }
        } finally {
            cursor.close();
        }
        return servers;
    }

    public void create(Server server) {
        Document doc = new Document("id", server.getId())
                .append("name", server.getName())
                .append("language", server.getLanguage())
                .append("framework", server.getFramework());
        collection.insertOne(doc);
    }

    public void delete(String id) {
        collection.deleteOne(new Document("id", id));
    }
}
