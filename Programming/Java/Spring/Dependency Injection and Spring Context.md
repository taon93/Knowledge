## Dependency injection:

### Definiowanie Beanów
    Spring ma kontekst aplikacji, w którym trzyma informację na temat Beanów- objektów klas utrzymywanych i zarządzanych przez framework w **IoCC** (inversion of control container).
Beany mogą mieć dependencje do innych beanów- w czasie wykonywania programu te dependencje są wstrzykiwane przez **IoCC**.
Zazwyczaj beany są tworzone za pomocą annotacji: najczęściej są to annotacje określające warstwę logiczną aplikacji, w której beany są używane:
**@Controller**, **@Service**, **@Repository**, jeżeli dana klasa nie jest elementem warstwy logicznej, a nadal ma być zarządzana przez Springa, to można ją zdefiniować za pomocą 
annotacji: **@Component** albo jeżeli chcemy mieć więcej kontroli nad inicjalizacją i zarządzaniem beanem można zdefiniować metodę z annotacją **@Bean** wewnątrz klasy konfiguracyjnej: 
**@Configuration**. 
    W kontekście beany są zdefiniowane w następujący sposób: 
1. Package qualified class name: nazwa klasy bezpośrednio definiująca beana
2. Konfiguracja zachowania: scope, lifecycle, callbacks etc.
3. Referencje beanów zależnych: jakie beany są potrzebne żeby bean działał
4. Inne ustawienia. 

### Bezpośrednie wyciąganie beanów z kontekstu springa: 
Beany można wyciągać z kontekstu za pomocą metody: `T getBean(String nameOfTheBean, Class<T> requiredType)`
Przykład: 
```aidl
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml") // constructors arguments are paths to XML configuration files
PetStroreService service = context.getBean("petStore", PetStoreService.class); // nameOfTheBean: "petStore" coresponds with <bean id="petStore... in XML configuration file
```
> Używanie takiej metody wyciągania beanów nie jest polecane: tworzy to dependencję do kontekstu springa samego w sobie

### Definiowanie beanów używanych jako dependencja: 
    Jeżeli jest więcej niż jeden bean danego typu (interface przekazywany jako dependencja który ma dwie implementacje) to w przypadku inicjalizacji aplikacji,
Spring zareaguje błędem, jako że nie będzie w stanie rozróżnić, który z nich powinien być użyty: są dwie metody, żeby sprecyzować, który bean powinien być wstrzyknięty:
1. **@Primary**: ta adnotacja informuje, że dana implementacja jest defaultowa: w przypadku więcej niż jednej możliwej, bean, który jest przez nią oznaczony, będzie tym wybranym.
2. Zastosowanie kwalifikatorów: można zdefiniować, który bean chcemy wstrzyknąć poprzez adnotację: **@Qualifier** (można ją zdefiniować przy argumencie wysyłanym do konstruktora, settera, albo bezpośrednio przy polu).
Ta adnotacja przyjmuje argument, który jest nazwą kwalifikatora, by default jest to nazwa funkcji pisana małą literą (żeby wstrzyknąć obiekt, klasy MyBeanA powinniśmy użyć: `@Qualifier("myBeanA")`),
ale można to zmienić przez zdefiniowanie nazwy beana w jego definicji na przykład: `@Service("myCustomBeanName")`, ta nazwa nadpisuje domyślną. 
3. Zastosowanie profili- poniżej

> Setter based injection of dependencies: be sure to Autowire setter and not the field: if not - setter will be not used

### Spring profiles: 
    `@Profile(<Name>)` - to adnotacja na poziomie klasy, przypisywana do definicji klasy, lub konfiguracji lub jako adnotacja na poziomie metody w klasie konfiguracji - przy definiowaniu beanów. 
Pozwala na uruchomienie aplikacji z określoną konfiguracją: w application properties/yml definiujemy jaki profile będzie aktualnie używany: `spring.profiles.active=dev`
informujemy tym samym kontekst, że w czasie komponent skanu, w przypadku kiedy jakiś bean jest zdefiniowany przez @Profile, to interesują nas tylko takie beany które mają nazwę tożsamą z tą zdefiniowaną
w `spring.profiles.acitve` ( w tym przypadku "dev"). Do jednego beanu może być przypisane więcej niż jeden profil np: `@Profiles("dev"), @Profile("prod"), @Profile("test") lub @Profile({"dev", "test", "prod"})
- ten bean będzie używany dla każdego z tych profili.
Inną metodą przypisywania profili (oprócz definiowania ich w application.properties/yml), to:
1. przekazywanie aktywnego profilu podczas odpalania aplikacji: jako zmiennej środowiskowej
2. przekazywanie w adnotacji: @ActiveProfile
3. ustawianie wewnątrz aplikacji: 
```java
public static void main(String[] args) {
    SpringApplication app = new SpringApplication(MyApplication.class);
    ConfigurableEnvironment env = app.run(args).getEnvironment();
    env.setActiveProfiles("dev");
}
```
    Jest też coś takiego jak profil domyśly, jeżeli żaden profil nie jest wybrany, bean który będzie stworzony to ten oznaczony przez `@Profile("default")`

### Spring bean lifecycle: 
By default beany w springu są konfigurowane jako singletony: jest tylko jedna instancja, która jest wstrzykiwana jako dependencja, inną metodą jest tworzenie nowej
instancji za każdym razem. Kontrola Inicjalizacji/destrukcji beanów:
1. spring udostępnia kilka miejsc w które można się wpiąć (hooks) podczas inicjalizacji/destrukcji beana.
2. Adnotacje metod: @PostConstruct/PreDestroy
3. BeanPostProcesor: można stworzyć objekt implementujący interface BeanPostProcessor, ma on dwie metody: postProcessBeforeInitialization, postProcessAfterInitialization
jest on często używany do wstrzykiwania konfiguracji do objektów z zewnątrz (3rd party), te metody są wołane globalnie: dla wszystkich obiektów wewnątrz kontekstu. Zazwyczaj warto
w nich zaimplemntować type checking.
4. Spring "Aware" interfaces
