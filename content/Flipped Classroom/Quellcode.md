Quellcode für das Video zum [[Flipped Classroom]]:
```python
from manim import *
from manim_voiceover import VoiceoverScene
from manim_voiceover.services.azure import AzureService

def fade_out(scene: Scene):
    animations = []
    for mobject in scene.mobjects:
        animations.append(FadeOut(mobject))
    scene.play(*animations)

class FracSector(VGroup):
    def __init__(self, numerator: int, denominator: int, color_part=BLUE, color_full=YELLOW, radius=1.0, mask=None, **kwargs):
        self.mobs = []
        circle = Circle()
        start = PI/2
        angle = -2*PI / denominator
        if mask == None:
            for i in range(numerator, denominator):
                self.mobs.append(ArcPolygon([0,0,0], circle.point_at_angle(start), circle.point_at_angle(start + angle),
                                    color=color_full, fill_opacity=0.1, stroke_opacity=0.3,
                                    arc_config=[{'angle': 0, 'color': color_full, 'fill_opacity': 0}, {'radius': -radius, 'color': color_full, 'fill_opacity': 0}, {'angle': 0, 'color': color_full, 'fill_opacity': 0}]))
                start += angle
            for i in range(0, numerator):
                self.mobs.append(ArcPolygon([0,0,0], circle.point_at_angle(start), circle.point_at_angle(start + angle),
                                    color=color_part, fill_opacity=0.5,
                                    arc_config=[{'angle': 0, 'color': color_part, 'fill_opacity': 0}, {'radius': -radius, 'color': color_part, 'fill_opacity': 0}, {'angle': 0, 'color': color_part, 'fill_opacity': 0}]))
                start += angle
        else:
            for bool in mask:
                if bool:
                    self.mobs.append(ArcPolygon([0,0,0], circle.point_at_angle(start), circle.point_at_angle(start + angle),
                                    color=color_part, fill_opacity=0.5,
                                    arc_config=[{'angle': 0, 'color': color_part, 'fill_opacity': 0}, {'radius': -radius, 'color': color_part, 'fill_opacity': 0}, {'angle': 0, 'color': color_part, 'fill_opacity': 0}]))
                    start += angle
                else:
                    self.mobs.append(ArcPolygon([0,0,0], circle.point_at_angle(start), circle.point_at_angle(start + angle),
                                    color=color_full, fill_opacity=0.1, stroke_opacity=0.3,
                                    arc_config=[{'angle': 0, 'color': color_full, 'fill_opacity': 0}, {'radius': -radius, 'color': color_full, 'fill_opacity': 0}, {'angle': 0, 'color': color_full, 'fill_opacity': 0}]))
                    start += angle
        super().__init__(*self.mobs, **kwargs)

class CombinedScene(VoiceoverScene):
    def construct(self):
        scenes = [
            TitelSzene, KreisTeilungSzene, Bruchteile, Uebung01,
            Bruch, Uebung02, Zusammenfassung]
        for scene in scenes:
            scene.construct(self)
            fade_out(self)

class TitelSzene(VoiceoverScene):
    def construct(self):
        self.set_speech_service(AzureService(
            voice="de-DE-KillianNeural"
        ))
        
        # Titeltext
        titel_text = Text("Kapitel 1: Rationale Zahlen", font_size=54)
        untertitel_text = Text("1.1 Bruchteile und Bruchzahlen", font_size=36)

        # Animationen
        with self.voiceover(text="Herzlich Willkommen zum Lernvideo zum Kapitel 1: Rationale Zahlen."):
            self.play(Write(titel_text))
        with self.voiceover(text="Abschnit 1: Bruchteile und Bruchzahlen"):
            self.play(Write(untertitel_text), titel_text.animate.shift(UP))
        self.wait(0.5)
        
class KreisTeilungSzene(VoiceoverScene):
    def construct(self):
        self.set_speech_service(AzureService(
            voice="de-DE-KillianNeural"
        ))
        teilungen = [2, 3, 4, 8]
        # Kreis erstellen
        color = YELLOW
        kreis = Circle(color=color, fill_opacity=0.5)
        text_kuchen = Text("Stell dir vor, du möchtest gerne eine Pizza essen:", font_size=24).next_to(kreis, 2*UP)
        teiltext = Text("Dafür teilst du die Pizza in mehrere gleichgroße Teile.", font_size=24).next_to(kreis, 2*DOWN)
        
        text_teilungen = [
            Text("Dafür teilst du die Pizza in zwei gleichgroße Teile.", font_size=24),
            Text("Dafür teilst du die Pizza in drei gleichgroße Teile.", font_size=24),
            Text("Dafür teilst du die Pizza in vier gleichgroße Teile.", font_size=24),
            Text("Dafür teilst du die Pizza in acht gleichgroße Teile.", font_size=24),
        ]

        # Kreis in mehrere Sektoren teilen
        vgs_teilungen = []
        for teilung, text in zip(teilungen, text_teilungen):
            sector = FracSector(teilung, teilung, color_part=YELLOW)
            text.next_to(sector, 2*DOWN)
            vgs_teilungen.append(sector)

        # Animationen
        with self.voiceover(text="Stell dir vor, du möchtest gerne eine Pizza essen.") as tracker:
            self.play(Create(kreis), Write(text_kuchen), run_time=tracker.duration)
        with self.voiceover(text="Du schneidest dafür die Pizza in mehrere gleich große Stücke.") as tracker:
            self.play(Write(teiltext), run_time=tracker.duration)
        
        prev = kreis
        prev_text = teiltext
        bms = ['B', 'C', 'D', 'E']
        with self.voiceover(text="""Je nach Lust und Laune kannst du sie zum Beispiel <bookmark mark='A'/>in zwei, <bookmark mark='B'/>drei, <bookmark mark='C'/>vier oder <bookmark mark='D'/>acht Stücke <bookmark mark='E'/>schneiden.""") as tracker:
            self.wait_until_bookmark('A')
            for vg, text, bm in zip(vgs_teilungen, text_teilungen, bms):
                self.play(ReplacementTransform(prev, vg), ReplacementTransform(prev_text, text), run_time=tracker.time_until_bookmark(bm, limit=1))
                prev, prev_text = vg, text
                self.wait_until_bookmark(bm)
        self.wait(0.5)

class Bruchteile(VoiceoverScene):
    def construct(self):
        self.set_speech_service(AzureService(
            voice="de-DE-KillianNeural"
        ))
        
        teilungen = [2, 3, 4, 8]
        color = YELLOW
        
        # Texte
        text_teilungen = [
            Text("Zwei Teile:", font_size=24),
            Text("Drei Teile:", font_size=24),
            Text("Vier Teile:", font_size=24),
            Text("Acht Teile:", font_size=24),
        ]
        text_name = [
            Text("Eine Hälfte", color=BLUE, font_size=24),
            Text("Ein Drittel", color=BLUE, font_size=24),
            Text("Ein Viertel", color=BLUE, font_size=24),
            Text("Ein Achtel", color=BLUE, font_size=24)
        ]
        text_bruch = [
            Tex(r"$\frac{1}{2}$", color=BLUE, font_size=96),
            Tex(r"$\frac{1}{3}$", color=BLUE, font_size=96),
            Tex(r"$\frac{1}{4}$", color=BLUE, font_size=96),
            Tex(r"$\frac{1}{8}$", color=BLUE, font_size=96)
        ]
        
        # Kreis in mehrere Sektoren teilen
        kreis = Circle(color=color, fill_opacity=0.5)
        vgs_teilungen = VGroup()
        vgs_teilungen_faded = VGroup()
        teile = VGroup()
        for teilung in teilungen:
            # Ganzes
            vg = FracSector(teilung, teilung, color_part=YELLOW)
            vgs_teilungen.add(vg)
            # Fadeout (Nichts)
            vg = FracSector(0, teilung, color_full=YELLOW)
            vgs_teilungen_faded.add(vg)
            # Bruchteil
            vg = FracSector(1, teilung, color_full=YELLOW, color_part=BLUE)
            teile.add(vg)
        vgs_teilungen.arrange(buff=1)
        vgs_teilungen_faded.arrange(buff=1)
        teile.arrange(buff=1)
        
        # Texte:
        for vg, text, name, bruch in zip(vgs_teilungen, text_teilungen, text_name, text_bruch):
            text.next_to(vg, 2*UP)
            name.next_to(vg, DOWN)
            bruch.next_to(name, DOWN)           
            
        # Animationen:
        # Teilungen
        with self.voiceover(text="Hier siehst du noch mal die geteilten Pizzas mit zwei, drei, vier, beziehungsweise acht gleichgroßen Stücken.") as tracker:
            self.play(Create(vgs_teilungen), Write(VGroup(*text_teilungen)))
        
        # Bruchteile
        bms = ['B', 'C', 'D', 'E']
        self.play(ReplacementTransform(vgs_teilungen, vgs_teilungen_faded))
        
        with self.voiceover(text="""Die Namen dieser einzelnen Stücken kennst du wahrscheinlich schon. <bookmark mark='A'/>Wurde die Pizza in zwei Teile geteilt, nennen wir sie eine Hälfte. <bookmark mark='B'/>Besteht die Pizza aus drei Stücken, heißt eines davon "Drittel", <bookmark mark='C'/>bei vier Stücken heißt eines davon "Viertel". <bookmark mark='D'/>Eines der acht Stücke wird Achtel genannt.<bookmark mark='E'/>""") as tracker:
            self.wait_until_bookmark('A')
            for teil, text, alt, bm in zip(teile, text_name, vgs_teilungen_faded, bms):
                self.play(ReplacementTransform(alt, teil), Write(text))
                self.wait_until_bookmark(bm)
        self.wait(0.5)
        
        with self.voiceover(text="""In der Mathematik verwenden wir diese Symbole, die für <bookmark mark='A'/>eine Hälfte, <bookmark mark='B'/>ein Drittel, <bookmark mark='C'/> ein Viertel und <bookmark mark='D'/> ein Achtel stehen.<bookmark mark='E'/>""") as tracker:
            self.play(VGroup(*teile, *text_teilungen, *text_name).animate.shift(UP))
            for bruch, bm in zip(text_bruch, bms):
                bruch.shift(UP)
                self.play(Write(bruch))
                self.wait_until_bookmark(bm)
        with self.voiceover(text="Sieh dir diese Schreibweise noch einmal genau an, wir machen gleich eine Übungsaufgabe.") as tracker:
            self.wait(tracker.duration)
        self.wait(1.0)

class Uebung01(VoiceoverScene):
    def construct(self):
        self.set_speech_service(AzureService(
            voice="de-DE-KillianNeural"
        ))
        teilungen = [5, 6, 7, 12]
        cd_len = 3
        
        anweisung = VGroup(Text("Benenne die folgenden Anteile in natürlicher Sprache", font_size=24), 
            Text("und gib auch das mathematische Symbol an!", font_size=24)).arrange(DOWN, buff=0)
        
        loesung_text = [
            Text("Ein Fünftel", color=BLUE, font_size=24),
            Text("Ein Sechstel", color=BLUE, font_size=24),
            Text("Ein Siebtel", color=BLUE, font_size=24),
            Text("Ein Zwölftel", color=BLUE, font_size=24)
        ]
        loesung_bruch = [
            Tex(r"$\frac{1}{5}$", color=BLUE, font_size=96),
            Tex(r"$\frac{1}{6}$", color=BLUE, font_size=96),
            Tex(r"$\frac{1}{7}$", color=BLUE, font_size=96),
            Tex(r"$\frac{1}{12}$", color=BLUE, font_size=96)
        ]
        
        vgs = VGroup()
        for nenner in teilungen:
            vgs.add(FracSector(1, nenner))
        vgs.arrange(buff=1)
        anweisung.next_to(vgs, 2*UP)
        for vg, text, bruch in zip(vgs, loesung_text, loesung_bruch):
            text.next_to(vg, DOWN)
            bruch.next_to(text, DOWN)
        
        # Anims:
        with self.voiceover(text="Kommen wir also jetzt zu einer Übungsaufgabe. Bist du so bereit?"):
            self.wait(1.0)
        
        with self.voiceover(text="Benenne die folgenden Anteile in natürlicher Sprache und gib auch das mathematische Symbol für den Anteil an!") as tracker:
            self.play(Write(anweisung), Create(vgs))
        
        with self.voiceover(text="Pausiere nun das Video und beantworte die Aufgabe!"):
            self.wait(1.0)
        self.wait(1.0)
        
        # Countdown
        cd = Text(f"Auflösung in {cd_len} Sekunden", font_size=24).to_corner(UP + RIGHT)
        self.play(Write(cd, run_time=0.3))
        for i in range(cd_len-1, -1, -1):
            self.wait(0.7)
            new_cd = Text(f"Auflösung in {i} Sekunden", font_size=24).to_corner(UP + RIGHT)
            self.play(ReplacementTransform(cd, new_cd, run_time=0.3))
            cd = new_cd
        self.wait(0.7)
        
        with self.voiceover(text="Alles klar? Ich zeige dir jetzt die Lösung.") as tracker:
            self.play(RemoveTextLetterByLetter(cd), run_time=tracker.duration)
        
        # Lösung
        self.play(VGroup(anweisung, vgs).animate.shift(UP))
        VGroup(*loesung_text, *loesung_bruch).shift(UP)
        self.play(Write(VGroup(*loesung_text)), Write(VGroup(*loesung_bruch)))
        self.wait(1.0)
        with self.voiceover(text="Na? Alles richtig gelöst? Schreib gerne eine Frage in unser Forum!"):
            self.wait(1.0)
        self.wait(0.5)

class Bruch(VoiceoverScene):
    def construct(self):
        self.set_speech_service(AzureService(
            voice="de-DE-KillianNeural"
        ))
        nenner = 6
        
        kuchen = FracSector(1, 6)
        bruch = MathTex(r"\frac{1}{6}", color=BLUE, font_size=96)
        bruch_text = Text("Ein Sechstel", color=BLUE,  font_size=32)
        bruch_vg = VGroup(kuchen, bruch).arrange(buff=1)
        bruch_vgtext = VGroup(bruch_vg, bruch_text.next_to(bruch_vg, 2*DOWN)).to_edge(LEFT)
        
        titel = VGroup(Text("Das Symbol, das wir für Anteile", font_size=32),
                      MarkupText(f'an einem Ganzen verwenden, nennen wir <span fgcolor="{BLUE}">Bruch</span>.', font_size=32)).arrange(DOWN)
        
        nenner_text = Text("Nenner:\nDie Größe der Teilstücke", font_size=32, color = GOLD).shift(RIGHT+0.7*DOWN)
        zaehler_text = Text("Zähler:\nDie Anzahl gleich großer Teilstücke", font_size=32, color = GOLD).shift(RIGHT+0.7*UP).align_to(nenner_text, LEFT)
        
        arrowz = Arrow(start=zaehler_text.get_left(),
                       end=zaehler_text.get_left()+1.5*LEFT, color=GOLD)
        arrown = Arrow(start=nenner_text.get_left(),
                       end=nenner_text.get_left()+1.5*LEFT, color=GOLD)
        
        with self.voiceover(text="Das Symbol, das wir für Anteile an einem Ganzen verwenden, nennen wir Bruch.") as tracker:
            self.play(Write(titel))
            self.play(titel.animate.to_edge(UP))
        self.play(Create(kuchen), Write(bruch_text), Write(bruch))
        
        with self.voiceover(text="Die obere Zahl des Bruchs nennen wir Zähler, da sie die Anzahl der Anteile angibt."):
            self.play(Write(zaehler_text), Create(arrowz))
        with self.voiceover(text="Die untere Zahl werden wir als Nenner bezeichnen. Sie gibt an, in wieviele gleich große Teilstücke das Ganze zu Beginn geteilt wurde."):        
            self.play(Write(nenner_text), Create(arrown))
            
        with self.voiceover(text="Der horizontale Strich zwischen Zähler und Nenner wird übrigens passenderweise Bruchstrich genannt."):
            self.wait(1.0)
        
        self.wait(0.8)

        with self.voiceover(text="Wir haben bisher nur Brüche betrachtet, die aus einem Teilstück bestehen. Bruchteile aus mehr als einem Teilstück werden beschrieben, indem die Anzahl der Teilstücke in den Zähler des Bruchs geschrieben wird."):
            self.wait(0.5)
        
        for z, s in zip([2, 3, 4, 5, 6, 3, 1], ["Zwei", "Drei", "Vier", "Fünf", "Sechs", "Drei", "Ein"]):
            bruch_neu = MathTex(r"\frac{"+ str(z) + r"}{6}", color=BLUE, font_size=96).align_to(bruch, LEFT)
            kuchen_neu = FracSector(z,6).align_to(kuchen, LEFT)
            text_neu = Text(s + " Sechstel", color=BLUE, font_size=32).align_to(bruch_text, LEFT+DOWN)
            self.play(ReplacementTransform(bruch, bruch_neu), ReplacementTransform(kuchen, kuchen_neu), ReplacementTransform(bruch_text, text_neu))
            bruch, kuchen, bruch_text = bruch_neu, kuchen_neu, text_neu
            self.wait(0.7)
        
        with self.voiceover(text="Lass uns noch eine Übung machen!"):
            self.wait(0.3)

class Uebung02(VoiceoverScene):
    def construct(self):
        self.set_speech_service(AzureService(
            voice="de-DE-KillianNeural"
        ))
        teilungen = [5, 6, 7, 12]
        masken = [
            [True, True, False, True, True],
            [True, False, True, False, True, False],
            [False, False, True, False, False, True, False],
            [True, True, False, True, True, False, True, False, False, True, True, False]
        ]
        cd_len = 3
        
        anweisung = VGroup(Text("Gib die Brüche an, die den Anteil der blauen Flächen", font_size=24), 
            Text("an der Gesamtfläche beschreiben!", font_size=24)).arrange(DOWN, buff=0)
        
        loesung_text = [
            Text("Vier Fünftel", color=BLUE, font_size=24),
            Text("Drei Sechstel", color=BLUE, font_size=24),
            Text("Zwei Siebtel", color=BLUE, font_size=24),
            Text("Sieben Zwölftel", color=BLUE, font_size=24)
        ]
        loesung_bruch = [
            Tex(r"$\frac{4}{5}$", color=BLUE, font_size=96),
            Tex(r"$\frac{3}{6}$", color=BLUE, font_size=96),
            Tex(r"$\frac{2}{7}$", color=BLUE, font_size=96),
            Tex(r"$\frac{7}{12}$", color=BLUE, font_size=96)
        ]
        
        vgs = VGroup()
        for nenner, maske in zip(teilungen, masken):
            vgs.add(FracSector(0, nenner, mask=maske))
        vgs.arrange(buff=1)
        anweisung.next_to(vgs, 2*UP)
        for vg, text, bruch in zip(vgs, loesung_text, loesung_bruch):
            text.next_to(vg, DOWN)
            bruch.next_to(text, DOWN)
        
        # Anims:
        with self.voiceover(text="Bist du bereit, deine Kenntnisse zu Brüchen zu beweisen?"):
            self.wait(1.0)
        
        with self.voiceover(text="Gib die Brüche an, die den Anteil der blauen Fächen an der Gesamtfläche beschreiben!") as tracker:
            self.play(Write(anweisung), Create(vgs))
            
        with self.voiceover(text="Pausiere nun das Video und beantworte die Aufgabe!"):
            self.wait(1.0)
        self.wait(1.0)

        # Countdown
        cd = Text(f"Auflösung in {cd_len} Sekunden", font_size=24).to_corner(UP + RIGHT)
        self.play(Write(cd, run_time=0.3))
        for i in range(cd_len-1, -1, -1):
            self.wait(0.7)
            new_cd = Text(f"Auflösung in {i} Sekunden", font_size=24).to_corner(UP + RIGHT)
            self.play(ReplacementTransform(cd, new_cd, run_time=0.3))
            cd = new_cd
        self.wait(0.7)
        
        with self.voiceover(text="Alles gelöst? Sehen wir uns die Lösung an.") as tracker:
            self.play(RemoveTextLetterByLetter(cd))
        
        # Lösung
        self.play(VGroup(anweisung, vgs).animate.shift(UP))
        VGroup(*loesung_text, *loesung_bruch).shift(UP)
        self.play(Write(VGroup(*loesung_text)), Write(VGroup(*loesung_bruch)))
        self.wait(1.0)
        with self.voiceover(text="Hast du alle Aufgaben geschafft? Schreib gerne eine Frage in unser Forum!"):
            self.wait(1.0)
        self.wait(0.5)

class Zusammenfassung(VoiceoverScene):
    def construct(self):
        self.set_speech_service(AzureService(
            voice="de-DE-KillianNeural"
        ))
        nenner = 6
        
        kuchen = FracSector(1, 6)
        bruch = MathTex(r"\frac{1}{6}", color=BLUE, font_size=96)
        bruch_text = Text("Ein Sechstel", color=BLUE,  font_size=32)
        bruch_vg = VGroup(kuchen, bruch).arrange(buff=1)
        bruch_vgtext = VGroup(bruch_vg, bruch_text.next_to(bruch_vg, 2*DOWN)).to_edge(LEFT)
        
        titel = VGroup(Text("Das Symbol, das wir für Anteile", font_size=32),
                      MarkupText(f'an einem Ganzen verwenden, nennen wir <span fgcolor="{BLUE}">Bruch</span>.', font_size=32)).arrange(DOWN)
        
        nenner_text = Text("Nenner:\nDie Größe der Teilstücke", font_size=32, color = GOLD).shift(RIGHT+0.7*DOWN)
        zaehler_text = Text("Zähler:\nDie Anzahl gleich großer Teilstücke", font_size=32, color = GOLD).shift(RIGHT+0.7*UP).align_to(nenner_text, LEFT)
        
        arrowz = Arrow(start=zaehler_text.get_left(),
                       end=zaehler_text.get_left()+1.5*LEFT, color=GOLD)
        arrown = Arrow(start=nenner_text.get_left(),
                       end=nenner_text.get_left()+1.5*LEFT, color=GOLD)
        
        with self.voiceover(text="Fassen wir also noch einmal zusammen! Bruchteile von einem Ganzen - zum Beispiel von einer Pizza - können mithilfe von Brüchen beschrieben werden. Ein Bruch besteht aus zwei Zahlen, dem Zähler und dem Nenner und einem horizontalen Bruchstrich dazwischen. Der Nenner eines Bruches ist die Anzahl gleichgroßer Stücke, in die das Ganze geteilt wurde. Der Zähler des Bruchs gibt die Anzahl solcher Stücke an, die wir betrachten."):
            self.play(Write(titel.to_edge(UP)))
            self.play(Create(kuchen), Write(bruch_text), Write(bruch))
            self.play(Write(zaehler_text), Create(arrowz), Write(nenner_text), Create(arrown))
            
        with self.voiceover(text="Solltest du Fragen zum Thema haben, kannst du eine Nachricht in unser Forum schreiben. Bis zum nächsten mal!"):
            self.wait(1.0)
        self.wait(1.0)
```