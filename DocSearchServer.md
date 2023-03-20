```
import java.io.File;
import java.io.IOException;
import java.net.URI;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.List;
import java.util.Collections;
import java.util.regex.Pattern;
import java.util.regex.Matcher;
import org.apache.commons.text.similarity.LevenshteinDistance;
import java.util.Map;
import java.util.HashMap;

class FileHelpers {
    static List<File> getFiles(Path start) throws IOException {
        File f = start.toFile();
        List<File> result = new ArrayList<>();
        if (f.isDirectory()) {
            File[] paths = f.listFiles();
            for (File subFile : paths) {
                result.addAll(getFiles(subFile.toPath()));
            }
        } else {
            result.add(start.toFile());
        }
        return result;
    }

    static String readFile(File f) throws IOException {
        return new String(Files.readAllBytes(f.toPath()));
    }
}

    class Handler implements URLHandler {
        Path base;

        Handler(String directory) throws IOException {
            this.base = Paths.get(directory);
        }

        public String handleRequest(URI url) throws IOException {
        List<File> paths = FileHelpers.getFiles(this.base);
        if (url.getPath().equals("/")) {
            return String.format("There are %d total files to search.", paths.size());
        } else if (url.getPath().equals("/search")) {
            // Parse query parameters
            Map<String, String> parameters = new HashMap<>();
            for (String param : url.getQuery().split("&")) {
                String[] keyValue = param.split("=");
                if (keyValue.length == 2) {
                    parameters.put(keyValue[0], keyValue[1]);
                }
            }

            String query = parameters.get("q");
            if (query == null || query.isEmpty()) {
                return "No search query provided.";
            }

            // Set default pagination values
            int page = Integer.parseInt(parameters.getOrDefault("page", "1"));
            int pageSize = Integer.parseInt(parameters.getOrDefault("page_size", "10"));

            // Perform search and filter results based on pagination
            List<String> foundPaths = new ArrayList<>();
            for (File f : paths) {
                String fileContent = FileHelpers.readFile(f);
                if (parameters.getOrDefault("ci", "false").equals("true")) {
                    fileContent = fileContent.toLowerCase();
                    query = query.toLowerCase();
                }

                if (parameters.getOrDefault("regex", "false").equals("true")) {
                    Pattern pattern = Pattern.compile(query);
                    Matcher matcher = pattern.matcher(fileContent);
                    if (matcher.find()) {
                        foundPaths.add(f.toString());
                    }
                } else if (parameters.getOrDefault("fuzzy", "false").equals("true")) {
                    String[] words = fileContent.split("\\s+");
                    LevenshteinDistance ld = new LevenshteinDistance(Integer.parseInt(parameters.getOrDefault("max_dist", "2")));
                    for (String word : words) {
                        if (ld.apply(word, query) != -1) {
                            foundPaths.add(f.toString());
                            break;
                        }
                    }
                } else {
                    if (fileContent.contains(query)) {
                        foundPaths.add(f.toString());
                    }
                }
            }

            // Calculate pagination information and filter results
            int totalResults = foundPaths.size();
            int start = (page - 1) * pageSize;
            int end = Math.min(start + pageSize, totalResults);
            foundPaths = foundPaths.subList(start, end);

            // Construct response
            String result = String.join("\n", foundPaths);
            int totalPages = (int) Math.ceil((double) totalResults / pageSize);
            String paginationInfo = String.format("Showing page %d of %d", page, totalPages);
            return String.format("Found %d paths:\n%s\n%s", foundPaths.size(), result, paginationInfo);
        } else {
            return "Invalid request path.";
        }
    }
}

class DocSearchServer {
    public static void main(String[] args) throws IOException {
        if (args.length == 0) {
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler("./written_2/"));
    }
}
```
