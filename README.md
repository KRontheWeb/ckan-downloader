
# CKAN Downloader for Amsterdam Data
This is a simple downloader for all data hosted through the Amsterdam city data portal (<http://data.amsterdam.nl>). Since this is a CKAN instance, we will use the CKAN API to retrieve the JSON description of the data, and use the `url` of the JSON object to download the data (if possible)


    import json
    import requests

* Do an HTTP GET against the API to retrieve all datasets (limited to 10000, which is way more than the CKAN contains). 
* Take the JSON representation of the response and convert it to a Python dictionary.
* Take the `results` element of the JSON object (a Python dictionary)


    ckan_response = requests.get('http://data.amsterdam.nl/api/search/dataset?all_fields=1&offset=0&limit=10000')


    ckan_json = ckan_response.json()


    results = ckan_json['results']

* Loop over all results
* For every result, check whether it is in a format that we can understand
* If so, retrieve it by doing a GET against the `url` of the resource
* ... and save it to the current directory.


    for r in results:
        rj = json.loads(r['data_dict'])
        
        for resource in rj['resources']:
            if resource['format'] in ['JSON','api','XLS','CSV','ZIP'] :
                print u"Retrieving {}".format(resource['name'])
                resource_data_response = requests.get(resource['url'])
                resource_filename = u"{}-{}.{}".format(resource['id'],resource['name'].replace(' ','_'),resource['format'].lower())
                
                try :
                    with open(resource_filename,'wb') as resource_file:
                        print u"Writing to {}".format(resource_filename)
                        resource_file.write(resource_data_response.content)
                except:
                    print u"Error while writing {}".format(resource_filename)

    Retrieving Leegstande kantoren (XLS)
    Writing to 947a51b8-f2cc-44a3-bc96-509246daf95b-Leegstande_kantoren_(XLS).xls
    Retrieving aanhoudingen in de regio amsterdam amstelland (Politie Eenheid Amsterdam)
    Writing to 7792f5ef-12f0-4335-a974-f42bff77fe53-aanhoudingen_in_de_regio_amsterdam_amstelland_(Politie_Eenheid_Amsterdam).xls
    Retrieving aanhoudingen in de regio amsterdam amstelland naar districten (Politie Eenheid Amsterdam)
    Writing to 6ebc78ca-caf2-41d7-947f-35f4c2f328bf-aanhoudingen_in_de_regio_amsterdam_amstelland_naar_districten_(Politie_Eenheid_Amsterdam).xls
    Retrieving aanhoudingen in de regio amsterdam amstelland naar geslacht leeftijd en districten (Politie Eenheid Amsterdam)
    Writing to 938345b5-eb0a-4e59-b54b-32b79f240352-aanhoudingen_in_de_regio_amsterdam_amstelland_naar_geslacht_leeftijd_en_districten_(Politie_Eenheid_Amsterdam).xls
    Retrieving aanhoudingen in de regio amsterdam amstelland naar resultaatgebieden (Politie Eenheid Amsterdam)
    Writing to 9b6c813c-e18d-4529-8bfc-45ba963b6fae-aanhoudingen_in_de_regio_amsterdam_amstelland_naar_resultaatgebieden_(Politie_Eenheid_Amsterdam).xls
    Retrieving aanhoudingen in verband met drugs in de regio amsterdam amstelland (Politie Eenheid Amsterdam)
    Writing to 94193dad-1c67-49ba-8a8e-b5d17cd11d56-aanhoudingen_in_verband_met_drugs_in_de_regio_amsterdam_amstelland_(Politie_Eenheid_Amsterdam).xls
    Retrieving aanhoudingen in verband met drugs in de regio amsterdam amstelland naar districten (Politie Eenheid Amsterdam)
    Writing to 19bd2cf0-58ab-43ee-8951-57fe3456040c-aanhoudingen_in_verband_met_drugs_in_de_regio_amsterdam_amstelland_naar_districten_(Politie_Eenheid_Amsterdam).xls
    Retrieving Aanhoudingen van minderjarigen (0-17 jaar) in de Regio Amsterdam-Amstelland naar leeftijdsgroepen en geslacht (Politie Eenheid Amsterdam)
    Writing to f930c5e8-2e90-4cf0-88f0-e6e1dc154c13-Aanhoudingen_van_minderjarigen_(0-17_jaar)_in_de_Regio_Amsterdam-Amstelland_naar_leeftijdsgroepen_en_geslacht_(Politie_Eenheid_Amsterdam).xls
    Retrieving geregistreerde misdrijven en ophelderingen (CBS)
    Writing to 8fff4e5f-65bb-4e59-a316-be5f80b7a6a8-geregistreerde_misdrijven_en_ophelderingen_(CBS).xls
    Retrieving geregistreerde misdrijven en ophelderingen (CBS)
    Writing to 05eba593-f619-4720-92c6-e3207f9f0fff-geregistreerde_misdrijven_en_ophelderingen_(CBS).xls
    Retrieving Misdrijven landelijke definitie in Amsterdam (Politie Eenheid Amsterdam)
    Writing to bca3def9-0242-4edc-abce-93eda6c743ef-Misdrijven_landelijke_definitie_in_Amsterdam_(Politie_Eenheid_Amsterdam).xls
    Retrieving Misdrijven landelijke definitie in Amsterdam naar stadsdelen en hoofdincidenten (Politie Eenheid Amsterdam)
    Writing to d1f70167-fadb-480f-ac2c-1f750b4a5ddc-Misdrijven_landelijke_definitie_in_Amsterdam_naar_stadsdelen_en_hoofdincidenten_(Politie_Eenheid_Amsterdam).xls
    Retrieving misdrijven landelijke definitie in de regio amsterdam amstelland (Politie Eenheid Amsterdam)
    Writing to d98cdca1-d2c8-4f4e-b8d2-4468961f753c-misdrijven_landelijke_definitie_in_de_regio_amsterdam_amstelland_(Politie_Eenheid_Amsterdam).xls
    Retrieving misdrijven landelijke definitie in de regio amsterdam amstelland (Politie Eenheid Amsterdam)
    Writing to 66c9e806-996d-43d4-93ef-e7f4375ea7e2-misdrijven_landelijke_definitie_in_de_regio_amsterdam_amstelland_(Politie_Eenheid_Amsterdam).xls
    Retrieving misdrijven landelijke definitie in de regio amsterdam amstelland naar districten (Politie Eenheid Amsterdam)
    Writing to dcfbca3c-1ab5-408b-be36-c7cb41d5aacd-misdrijven_landelijke_definitie_in_de_regio_amsterdam_amstelland_naar_districten_(Politie_Eenheid_Amsterdam).xls
    Retrieving misdrijven landelijke definitie in de regio amsterdam amstelland naar districten en hoofdincidenten (Politie Eenheid Amsterdam)
    Writing to bd66d981-fdbb-4f5c-ad0c-8811b11264c1-misdrijven_landelijke_definitie_in_de_regio_amsterdam_amstelland_naar_districten_en_hoofdincidenten_(Politie_Eenheid_Amsterdam).xls
    Retrieving misdrijven landelijke definitie in de regio amsterdam amstelland naar gemeenten (Politie Eenheid Amsterdam)
    Writing to 4067f379-c856-4fd5-9f3c-599c1cb6e0c6-misdrijven_landelijke_definitie_in_de_regio_amsterdam_amstelland_naar_gemeenten_(Politie_Eenheid_Amsterdam).xls
    Retrieving misdrijven landelijke definitie in de regio amsterdam amstelland naar gemeenten en hoofdincidenten (Politie Eenheid Amsterdam)
    Writing to 6aba49db-4053-47bf-9445-c6a61bb73caf-misdrijven_landelijke_definitie_in_de_regio_amsterdam_amstelland_naar_gemeenten_en_hoofdincidenten_(Politie_Eenheid_Amsterdam).xls
    Retrieving bevolking (CBS)
    Writing to 0aefebd6-3768-4a28-923a-cd4f14323f12-bevolking_(CBS).xls
    Retrieving bevolking naar geslacht (CBS)
    Writing to 658e3456-bf3c-4e76-9cf7-c0ed4772d297-bevolking_naar_geslacht_(CBS).xls
    Retrieving bevolking naar leeftijdsgroepen (CBS)
    Writing to 321e388d-bee3-48e4-affb-77ff587fa753-bevolking_naar_leeftijdsgroepen_(CBS).xls
    Retrieving bevolking naar leeftijdsgroepen en burgerlijke staat (CBS)
    Writing to c025b174-683b-4f03-aa70-9bb1e48a1d7b-bevolking_naar_leeftijdsgroepen_en_burgerlijke_staat_(CBS).xls
    Retrieving echtscheidende personen (CBS)
    Writing to a9504089-36e3-4f6c-9ce8-f87418c3678a-echtscheidende_personen_(CBS).xls
    Retrieving huwelijken (CBS)
    Writing to c4bb06df-7b4b-46fe-8d20-278849c5777c-huwelijken_(CBS).xls
    Retrieving huwende personen (CBS)
    Writing to c95ffcd6-0ebb-444b-808c-1717aea2e6bc-huwende_personen_(CBS).xls
    Retrieving fosfaatuitscheiding tijdens dierlijke mestproductie (CBS)
    Writing to dd986c9e-7814-463c-9df1-4f259a4b0d04-fosfaatuitscheiding_tijdens_dierlijke_mestproductie_(CBS).xls
    Retrieving geproduceerde dierlijke mest (CBS)
    Writing to e87b3f1b-cf8b-4c9f-90b9-43a9bf5eca84-geproduceerde_dierlijke_mest_(CBS).xls
    Retrieving graasdieren bedrijven met graasdieren en oppervlakte grasland (CBS)
    Writing to 00276a89-bb7a-49c0-9f23-c73306d6975b-graasdieren_bedrijven_met_graasdieren_en_oppervlakte_grasland_(CBS).xls
    Retrieving kali uitscheiding tijdens dierlijke mestproductie (CBS)
    Writing to 59b44768-685c-45a2-9c89-1635448b05fc-kali_uitscheiding_tijdens_dierlijke_mestproductie_(CBS).xls
    Retrieving meldingen met betrekking tot geluidshinder schiphol gebruiksjaar (CBS)
    Writing to d5797b10-4f70-4ca3-a105-66d155076a28-meldingen_met_betrekking_tot_geluidshinder_schiphol_gebruiksjaar_(CBS).xls
    Retrieving stikstofproductie tijdens dierlijke mestproductie (CBS)
    Writing to bf6807b7-a558-4aaf-accf-c3ac17f16d69-stikstofproductie_tijdens_dierlijke_mestproductie_(CBS).xls
    Retrieving Energielabels (CSV)
    Writing to b4a4d578-841f-44ad-9b28-d400b5b41a7d-Energielabels_(CSV).csv
    Retrieving stikstofdioxide in de buitenlucht naar stadsdelen jaargemiddelde van de uurgemiddelde metingen (GGD Amsterdam)
    Writing to 755747d1-5186-4641-8082-6a5a37bc538f-stikstofdioxide_in_de_buitenlucht_naar_stadsdelen_jaargemiddelde_van_de_uurgemiddelde_metingen_(GGD_Amsterdam).xls
    Retrieving amsterdamse verzorgingshuizen en verpleeghuizen naar aantal bedden en zorgregios (SIGRA)
    Writing to 356fd3a3-f08a-4274-bdb0-8d1354ff7de6-amsterdamse_verzorgingshuizen_en_verpleeghuizen_naar_aantal_bedden_en_zorgregios_(SIGRA).xls
    Retrieving bevolking van 15 74 jaar naar hoogst afgerond opleidingsniveau (CBS/bewerking OIS)
    Error while writing e61c8ded-7a29-4662-8f2f-5097c53240d1-bevolking_van_15_74_jaar_naar_hoogst_afgerond_opleidingsniveau_(CBS/bewerking_OIS).xls
    Retrieving bevolking van 15 74 jaar naar hoogst afgerond opleidingsniveau (CBS/bewerking OIS)
    Error while writing 7bb5ff17-fd05-46af-bb44-361a54fb9c77-bevolking_van_15_74_jaar_naar_hoogst_afgerond_opleidingsniveau_(CBS/bewerking_OIS).xls
    Retrieving Stadsdelen
    Writing to 5a9170f6-9958-4454-a4fb-072a709e892f-Stadsdelen.zip
    Retrieving Buurtcombinaties
    Writing to 7cf25148-c17e-4d35-9931-32f0b607b901-Buurtcombinaties.zip
    Retrieving Buurten
    Writing to 24052074-7d00-4e2f-86da-9a13d2d12220-Buurten.zip
    Retrieving Stadsdelen (bewoonde gebieden)
    Writing to fd03d305-5382-416d-85fe-020162a7d882-Stadsdelen_(bewoonde_gebieden).zip
    Retrieving Buurtcombinaties (bewoonde gebieden)
    Writing to 995921bd-96bc-46d3-b85b-d12ad8ff2887-Buurtcombinaties_(bewoonde_gebieden).zip
    Retrieving Buurten (bewoonde gebieden)
    Writing to ab77912c-8121-43e0-adb8-3a58172f3a50-Buurten_(bewoonde_gebieden).zip
    Retrieving contact met politie naar veiligheidsregios (CBS/VM 2014)
    Error while writing 19f1b4f7-9d7b-4ae2-947a-c50c4cc5dc0f-contact_met_politie_naar_veiligheidsregios_(CBS/VM_2014).xls
    Retrieving functioneren van de politie naar veiligheidsregios (CBS/VM 2014)
    Error while writing 1787c9f2-5f29-4002-83a9-a0935df389c2-functioneren_van_de_politie_naar_veiligheidsregios_(CBS/VM_2014).xls
    Retrieving fysieke verloedering naar veiligheidsregios (CBS/VM 2014)
    Error while writing 15a74685-7f1b-4cef-a155-351e210fbed6-fysieke_verloedering_naar_veiligheidsregios_(CBS/VM_2014).xls
    Retrieving geregistreerde diefstallen (CBS)
    Writing to 473348ec-af9c-4194-9c72-b33eb45cadad-geregistreerde_diefstallen_(CBS).xls
    Retrieving geregistreerde misdrijven (CBS)
    Writing to 533b77a3-6f37-421b-8f8a-0bec0ed10970-geregistreerde_misdrijven_(CBS).xls
    Retrieving geregistreerde misdrijven en ophelderingen (CBS)
    Writing to 2d740639-7ae3-4d50-ac23-a49fa11032b0-geregistreerde_misdrijven_en_ophelderingen_(CBS).xls
    Retrieving onveiligheidsbeleving naar veiligheidsregios (CBS/VM 2014)
    Error while writing 6454c874-8bc9-4587-b3a6-e031f6c913d3-onveiligheidsbeleving_naar_veiligheidsregios_(CBS/VM_2014).xls
    Retrieving preventie naar veiligheidsregios (CBS/VM 2014)
    Error while writing 53fa0c11-5319-41e5-9214-35d15fc26bf6-preventie_naar_veiligheidsregios_(CBS/VM_2014).xls
    Retrieving slachtofferschap criminaliteit naar veiligheidsregios (CBS/VM 2014)
    Error while writing c8f4f0da-48e7-44ef-bfd2-5c959d38f8c5-slachtofferschap_criminaliteit_naar_veiligheidsregios_(CBS/VM_2014).xls
    Retrieving sociale overlast naar veiligheidsregios (CBS/VM 2014)
    Error while writing b268220c-7d99-498d-81a4-8858584f8a26-sociale_overlast_naar_veiligheidsregios_(CBS/VM_2014).xls
    Retrieving verkeersoverlast naar veiligheidsregios (CBS/VM 2014)
    Error while writing 1cb3129e-5749-443d-9091-7edf069a078a-verkeersoverlast_naar_veiligheidsregios_(CBS/VM_2014).xls
    Retrieving sterfte naar doodsoorzaken (CBS)
    Writing to 30b4ae42-0e7c-4444-875a-6e36d6dd042d-sterfte_naar_doodsoorzaken_(CBS).xls
    Retrieving uitkomsten gezondheidsmonitor (CBS)
    Writing to 81ce2c10-b1be-4438-bf2f-e587b7f99839-uitkomsten_gezondheidsmonitor_(CBS).xls
    Retrieving uitkomsten gezondheidsmonitor per gemeente (CBS)
    Writing to 26cdc2bf-7ef7-4cc2-abc2-0455a7794b36-uitkomsten_gezondheidsmonitor_per_gemeente_(CBS).xls
    Retrieving Bekendmakingen horecavergunningen met link naar bekendmaking
    Writing to 45b9ac18-4add-4251-893e-3fffc39b41cc-Bekendmakingen_horecavergunningen_met_link_naar_bekendmaking.csv
    Retrieving Misdrijven landelijke definitie in Amsterdam
    Writing to 08a780db-79f8-4e8f-b001-e0b14a7da19f-Misdrijven_landelijke_definitie_in_Amsterdam.xls
    Retrieving Misdrijven landelijke definitie in Amsterdam naar resultaatgebieden
    Writing to c96372b6-f17c-4b34-8022-1d99f952e31e-Misdrijven_landelijke_definitie_in_Amsterdam_naar_resultaatgebieden.xls
    Retrieving baangebruik amsterdam airport schiphol gebruiksjaar (Bewoners Aanspreekpunt Schiphol)
    Writing to e1121eef-7448-4b82-a75c-d06ae4f7604e-baangebruik_amsterdam_airport_schiphol_gebruiksjaar_(Bewoners_Aanspreekpunt_Schiphol).xls
    Retrieving goederenoverslag in de west europese havens hamburg le havre x mln ton (Haven Amsterdam)
    Writing to 6f395df4-76af-4d5f-b279-a105089e59f8-goederenoverslag_in_de_west_europese_havens_hamburg_le_havre_x_mln_ton_(Haven_Amsterdam).xls
    Retrieving Goederenoverslag naar soort in de Amsterdamse haven (Haven Amsterdam)
    Writing to b2571124-ae17-4e78-9a27-4752159d6d22-Goederenoverslag_naar_soort_in_de_Amsterdamse_haven_(Haven_Amsterdam).xls
    Retrieving Kerncijfers Amsterdamse haven (Haven Amsterdam)
    Writing to 94b4bccf-edb3-4ee9-90c5-ee24285a4d73-Kerncijfers_Amsterdamse_haven_(Haven_Amsterdam).xls
    Retrieving luchtvervoer op amsterdam airport schiphol (Amsterdam Airport Schiphol)
    Writing to b7f389ad-e544-400e-9a35-db9de4b15081-luchtvervoer_op_amsterdam_airport_schiphol_(Amsterdam_Airport_Schiphol).xls
    Retrieving passagiers op amsterdam airport schiphol (Amsterdam Airport Schiphol)
    Writing to c5edfe42-5b45-4ad6-83b9-bbcab0d71945-passagiers_op_amsterdam_airport_schiphol_(Amsterdam_Airport_Schiphol).xls
    Retrieving passagiersschepen en passagiers in de amsterdamse zeehavens (Haven Amsterdam/Marechaussee/DFDS/Amsterdam Cruise Port)
    Error while writing 3eed6df7-c041-4140-b4a0-23cb40536bdf-passagiersschepen_en_passagiers_in_de_amsterdamse_zeehavens_(Haven_Amsterdam/Marechaussee/DFDS/Amsterdam_Cruise_Port).xls
    Retrieving passagiersvervoer nederlandse luchthavens (Amsterdam Airport Schiphol/CBS)
    Error while writing e792fd78-17cf-4508-9e54-7f7cd654d3cf-passagiersvervoer_nederlandse_luchthavens_(Amsterdam_Airport_Schiphol/CBS).xls
    Retrieving vliegtuigbewegingen op amsterdam airport schiphol (Amsterdam Airport Schiphol)
    Writing to 5e0ce3a9-9206-4040-932d-00231c0e0a60-vliegtuigbewegingen_op_amsterdam_airport_schiphol_(Amsterdam_Airport_Schiphol).xls
    Retrieving Winkeliersverenigingen (XLS)
    Writing to 042bd77f-a501-452c-a3f5-ec445d938107-Winkeliersverenigingen_(XLS).xls
    Retrieving woningvoorraad (CBS)
    Writing to d7f3d292-914f-40e7-ab2f-a74b83551c7c-woningvoorraad_(CBS).xls
    Retrieving woningvoorraad en mutaties in de woningvoorraad (CBS)
    Writing to 0fc56890-42ad-490c-a576-16587b7e269d-woningvoorraad_en_mutaties_in_de_woningvoorraad_(CBS).xls
    Retrieving bevolking van 15 74 jaar naar hoogst afgerond opleidingsniveau (CBS/bewerking OIS)
    Error while writing 07afee57-e6f9-4dbb-93eb-e4c6820dd75d-bevolking_van_15_74_jaar_naar_hoogst_afgerond_opleidingsniveau_(CBS/bewerking_OIS).xls
    Retrieving Deeltijdstudenten in het hoger beroepsonderwijs naar studierichting en geslacht (OCW/DUO)
    Error while writing 5aa55ed9-be2d-4067-9957-485c1bc5280d-Deeltijdstudenten_in_het_hoger_beroepsonderwijs_naar_studierichting_en_geslacht_(OCW/DUO).xls
    Retrieving eindexamenkandidaten en geslaagden naar examenjaar (Onderwijsinspectie)
    Writing to 6bf5514d-afcb-41f5-abbe-601941efa862-eindexamenkandidaten_en_geslaagden_naar_examenjaar_(Onderwijsinspectie).xls
    Retrieving Geslaagden voor afsluitende examens in het HBO (OCW/DUO)
    Error while writing 79ac1e86-08f0-4a8b-b1cd-69b961c64c08-Geslaagden_voor_afsluitende_examens_in_het_HBO_(OCW/DUO).xls
    Retrieving Geslaagden voor afsluitende examens in het MBO (OCW/DUO)
    Error while writing 5c498818-0561-492a-8540-7928048dbfaf-Geslaagden_voor_afsluitende_examens_in_het_MBO_(OCW/DUO).xls
    Retrieving Geslaagden voor afsluitende universitaire examens (OCW/DUO)
    Error while writing aac961eb-ef93-4d65-9c47-db9f5d3c0caa-Geslaagden_voor_afsluitende_universitaire_examens_(OCW/DUO).xls
    Retrieving Groei van het aantal leerlingen en studenten naar type onderwijs (OCW/DUO)
    Error while writing 04d8efad-cc05-4b85-82b2-1ef79b713ae1-Groei_van_het_aantal_leerlingen_en_studenten_naar_type_onderwijs_(OCW/DUO).xls
    Retrieving Leerlingen basisonderwijs naar herkomstgroepering (afd. Onderwijs, Jeugd en Zorg/OIS)
    Error while writing 78414aa6-8d19-4518-8c1e-25711f538184-Leerlingen_basisonderwijs_naar_herkomstgroepering_(afd._Onderwijs,_Jeugd_en_Zorg/OIS).xls
    Retrieving Leerlingen en studenten in de derde klas van het VMBO (inclusief LWOO) naar studierichting en geslacht (OCW/DUO)
    Error while writing 85158869-8235-4d02-b8e1-d93223752d76-Leerlingen_en_studenten_in_de_derde_klas_van_het_VMBO_(inclusief_LWOO)_naar_studierichting_en_geslacht_(OCW/DUO).xls
    Retrieving Leerlingen en studenten naar type onderwijs (OCW/DUO)
    Error while writing b4877f76-46a2-4a23-96d8-b3d8ccabfc1f-Leerlingen_en_studenten_naar_type_onderwijs_(OCW/DUO).xls
    Retrieving Leerlingen in de derde klas van Amsterdamse scholen met voortgezet onderwijs naar herkomstgroepering en type onderwijs (OCW/DUO)
    Error while writing 1e06e743-ceff-41c6-adbc-eba49507a856-Leerlingen_in_de_derde_klas_van_Amsterdamse_scholen_met_voortgezet_onderwijs_naar_herkomstgroepering_en_type_onderwijs_(OCW/DUO).xls
    Retrieving Leerlingen in het (voortgezet) speciaal onderwijs (OCW/DUO)
    Error while writing aae55376-22d1-47d4-8f61-cdaf15bd474f-Leerlingen_in_het_(voortgezet)_speciaal_onderwijs_(OCW/DUO).xls
    Retrieving Leerlingen in het voortgezet onderwijs naar leerjaar en geslacht (OCW/DUO)
    Error while writing d67c1915-0ab1-46b9-bbcf-c5c11a21070f-Leerlingen_in_het_voortgezet_onderwijs_naar_leerjaar_en_geslacht_(OCW/DUO).xls
    Retrieving Leerlingen speciaal onderwijs naar herkomstgroepering (afd. Onderwijs, Jeugd en Zorg/OIS)
    Error while writing 2285a436-62be-4109-821b-44f5d5392452-Leerlingen_speciaal_onderwijs_naar_herkomstgroepering_(afd._Onderwijs,_Jeugd_en_Zorg/OIS).xls
    Retrieving Leerlingen voortgezet onderwijs naar herkomstgroepering (OCW/DUO)
    Error while writing 6bd291b3-eca0-4f24-bf87-f03d5eb36cb3-Leerlingen_voortgezet_onderwijs_naar_herkomstgroepering_(OCW/DUO).xls
    Retrieving mbo leerlingen naar instelling (OCW/DUO)
    Error while writing f984ca71-1d58-4408-9dbb-e91c5b02fdc7-mbo_leerlingen_naar_instelling_(OCW/DUO).xls
    Retrieving mbo leerlingen woonachtig in amsterdam naar geslacht niveau en leerweg (CBS)
    Writing to 537e56fd-5b65-4f3d-a52a-44c35cda788f-mbo_leerlingen_woonachtig_in_amsterdam_naar_geslacht_niveau_en_leerweg_(CBS).xls
    Retrieving mbo leerlingen woonachtig in amsterdam naar studierichting en geslacht (CBS)
    Writing to 359d4f3c-9e44-4f16-9899-0be1f95095b9-mbo_leerlingen_woonachtig_in_amsterdam_naar_studierichting_en_geslacht_(CBS).xls
    Retrieving Scholen (OCW/DUO)
    Error while writing f2cef53a-171d-493c-83d1-ffcd44a3babf-Scholen_(OCW/DUO).xls
    Retrieving Scholen en leerlingen in het speciaal basisonderwijs (OCW/DUO)
    Error while writing 9417a4ec-e5ad-4f2d-9619-d886b977e4f8-Scholen_en_leerlingen_in_het_speciaal_basisonderwijs_(OCW/DUO).xls
    Retrieving Scholen in het (voortgezet) speciaal onderwijs (OCW/DUO)
    Error while writing 52b97d3f-a14f-4238-9adf-2c381b2b7f94-Scholen_in_het_(voortgezet)_speciaal_onderwijs_(OCW/DUO).xls
    Retrieving Scholen naar type onderwijs en schooljaar (OCW/DUO)
    Error while writing 842b5129-2161-49d3-ae15-abb007cdfafd-Scholen_naar_type_onderwijs_en_schooljaar_(OCW/DUO).xls
    Retrieving Schoolverzuim 1) naar stadsdeel van vestiging van de school (OCW/DUO)
    Error while writing 3dbde1ac-a8e5-4adb-9bd0-6e1ac2152e40-Schoolverzuim_1)_naar_stadsdeel_van_vestiging_van_de_school_(OCW/DUO).xls
    Retrieving Schoolverzuim op Amsterdamse scholen naar schoolsoort (OCW/DUO)
    Error while writing eb268dc3-9b0a-4718-a0d4-d73c9646246f-Schoolverzuim_op_Amsterdamse_scholen_naar_schoolsoort_(OCW/DUO).xls
    Retrieving Studenten in het hoger beroepsonderwijs (OCW/DUO)
    Error while writing 006d8e24-1191-4799-abb8-4244ef6d0c99-Studenten_in_het_hoger_beroepsonderwijs_(OCW/DUO).xls
    Retrieving Studenten in het wetenschappelijk onderwijs (OCW/DUO)
    Error while writing ca6f9d25-e43f-468d-aa67-da8b80bb9cc5-Studenten_in_het_wetenschappelijk_onderwijs_(OCW/DUO).xls
    Retrieving Studenten in het wetenschappelijk onderwijs naar studierichting en inschrijvingsvorm (OCW/DUO)
    Error while writing d766536c-f70a-46b5-b2f9-96a612a11f92-Studenten_in_het_wetenschappelijk_onderwijs_naar_studierichting_en_inschrijvingsvorm_(OCW/DUO).xls
    Retrieving voltijd en duaalstudenten in het hoger beroepsonderwijs naar studierichting en geslacht (OCW/DUO)
    Error while writing a14a1054-f26f-4c1b-a6f6-e2808c80418c-voltijd_en_duaalstudenten_in_het_hoger_beroepsonderwijs_naar_studierichting_en_geslacht_(OCW/DUO).xls
    Retrieving bedrijfsvestigingen (LISA)
    Writing to 7a421fee-afeb-498d-9aff-7574c18fe647-bedrijfsvestigingen_(LISA).xls
    Retrieving bedrijfsvestigingen in de tertiaire sector (LISA)
    Writing to 47004c43-52c5-46e2-8193-f5b8eebb8371-bedrijfsvestigingen_in_de_tertiaire_sector_(LISA).xls
    Retrieving bedrijfsvestigingen naar sectoren (LISA)
    Writing to 803cdaed-b99c-4e42-8010-3734d8bfbe89-bedrijfsvestigingen_naar_sectoren_(LISA).xls
    Retrieving beloning van werknemers (CBS)
    Writing to cc6d3c38-6a99-4c45-b006-5217d9f2d879-beloning_van_werknemers_(CBS).xls
    Retrieving beroepsbevolking en arbeidsparticipatie (CBS)
    Writing to d2295dfe-28a0-4e33-8f6b-8e6033d526b6-beroepsbevolking_en_arbeidsparticipatie_(CBS).xls
    Retrieving bruto binnenlands product bbp marktprijzen (CBS)
    Writing to 3c6b5c48-d132-4799-bf4c-64771394a45b-bruto_binnenlands_product_bbp_marktprijzen_(CBS).xls
    Retrieving bruto binnenlands product bbp per hoofd van de bevolking (CBS)
    Writing to f8b4b965-bec9-4dde-8cca-239b1673206a-bruto_binnenlands_product_bbp_per_hoofd_van_de_bevolking_(CBS).xls
    Retrieving totaal toegevoegde waarde basisprijzen (CBS)
    Writing to d2a0cc69-3875-4515-9fef-b421e1aebdeb-totaal_toegevoegde_waarde_basisprijzen_(CBS).xls
    Retrieving volumegroei van de toegevoegde waarde basisprijzen (CBS)
    Writing to 8c8affdc-0c9f-48c2-93ad-b1e9aa86b900-volumegroei_van_de_toegevoegde_waarde_basisprijzen_(CBS).xls
    Retrieving volumegroei van het bruto binnenlands product bbp (CBS)
    Writing to 0a6f18e0-41d5-4500-832e-f6c166f8d15a-volumegroei_van_het_bruto_binnenlands_product_bbp_(CBS).xls
    Retrieving werkzame beroepsbevolking 15 74 jaar naar herkomstgroepering en onderwijsniveau (CBS)
    Writing to c505fd75-3edb-4a86-a2fd-5457b25d17e7-werkzame_beroepsbevolking_15_74_jaar_naar_herkomstgroepering_en_onderwijsniveau_(CBS).xls
    Retrieving werkzame personen (LISA)
    Writing to 0489ed8f-acd9-4c19-8f1e-fa6271fd8f9c-werkzame_personen_(LISA).xls
    Retrieving werkzame personen in de tertiaire sector (LISA)
    Writing to 162cd812-2f6c-43c5-b0f7-1886904064b1-werkzame_personen_in_de_tertiaire_sector_(LISA).xls
    Retrieving werkzame personen naar sectoren (LISA)
    Writing to 287aaba5-9c7b-4bbf-8c50-7701d3817925-werkzame_personen_naar_sectoren_(LISA).xls
    Retrieving werkzame personen naar soort banen en geslacht (LISA)
    Writing to 8ad181c0-b38a-41f8-9fef-ce5fe996b7f3-werkzame_personen_naar_soort_banen_en_geslacht_(LISA).xls
    Retrieving bevolking van 15 74 jaar naar hoogst afgerond opleidingsniveau (CBS/bewerking OIS)
    Error while writing 7ec882ea-9a42-4991-843a-c59433d7f2f7-bevolking_van_15_74_jaar_naar_hoogst_afgerond_opleidingsniveau_(CBS/bewerking_OIS).xls
    Retrieving Bevolking naar burgerlijke staat en geslacht (OIS)
    Writing to 0321a935-b4e2-4832-ae41-2bfe86bec73c-Bevolking_naar_burgerlijke_staat_en_geslacht_(OIS).xls
    Retrieving Bevolking naar herkomstgroepering (OIS)
    Writing to e552c30f-343e-4120-8e53-876570cb0fc2-Bevolking_naar_herkomstgroepering_(OIS).xls
    Retrieving Bevolking naar herkomstgroepering (OIS)
    Writing to 82a5f4f5-09cf-4b7e-a1d2-1b9362c90f7d-Bevolking_naar_herkomstgroepering_(OIS).xls
    Retrieving Bevolking naar herkomstgroepering (OIS)
    Writing to 0faa0b5f-bcbb-4b6d-9d7b-1d942ba13796-Bevolking_naar_herkomstgroepering_(OIS).xls
    Retrieving Bevolking naar herkomstgroepering en generatie (OIS)
    Writing to c50abcbf-4aaf-49b5-ae76-8601ac073e3e-Bevolking_naar_herkomstgroepering_en_generatie_(OIS).xls
    Retrieving Bevolking naar huishoudenstypen (OIS)
    Writing to 53e8fa93-216d-4e3a-b0e0-201761a8e29d-Bevolking_naar_huishoudenstypen_(OIS).xls
    Retrieving Bevolking naar huishoudenstypen (OIS)
    Writing to bb804f22-fdac-495a-9cb4-55c32886ea5d-Bevolking_naar_huishoudenstypen_(OIS).xls
    Retrieving Bevolking naar leeftijdsgroepen (afd. Ruimte en Duurzaamheid/OIS)
    Error while writing 266eed4b-5e10-4d7e-bcd4-ae99f9757700-Bevolking_naar_leeftijdsgroepen_(afd._Ruimte_en_Duurzaamheid/OIS).xls
    Retrieving bevolking naar leeftijdsgroepen (CBS/OIS)
    Error while writing 1d136b7a-8ace-4a31-8919-974d5060e4b3-bevolking_naar_leeftijdsgroepen_(CBS/OIS).xls
    Retrieving Bevolking naar leeftijdsgroepen en geslacht (OIS)
    Writing to 2ef82d36-4bb2-4e92-945d-aaa092db8211-Bevolking_naar_leeftijdsgroepen_en_geslacht_(OIS).xls
    Retrieving Bevolking naar leeftijdsgroepen en herkomstgroepering (OIS)
    Writing to 24fe9778-4b3b-4486-b4bc-8a7d13fd6fa2-Bevolking_naar_leeftijdsgroepen_en_herkomstgroepering_(OIS).xls
    Retrieving Bevolking naar leeftijdsgroepen en herkomstgroepering (afd. Ruimte en Duurzaamheid/OIS)
    Error while writing 02711104-18eb-4e6c-b5f4-7bf0cdcb02ca-Bevolking_naar_leeftijdsgroepen_en_herkomstgroepering_(afd._Ruimte_en_Duurzaamheid/OIS).xls
    Retrieving Bevolking naar meest voorkomende herkomstgroepering (meer dan 1.000 personen per groep) (OIS)
    Writing to f05c5db7-1480-42d6-945e-51ebdd99335d-Bevolking_naar_meest_voorkomende_herkomstgroepering_(meer_dan_1.000_personen_per_groep)_(OIS).xls
    Retrieving Bevolking naar meest voorkomende nationaliteiten (OIS)
    Writing to 3c66a9d9-de4c-47cf-bfbd-880891cf444b-Bevolking_naar_meest_voorkomende_nationaliteiten_(OIS).xls
    Retrieving Bevolking naar nationaliteiten (OIS)
    Writing to 488cf3bd-c00f-40b4-8c0e-e7db24cb3ce1-Bevolking_naar_nationaliteiten_(OIS).xls
    Retrieving bevolking naar nationaliteiten (CBS/OIS)
    Error while writing 66c11ff2-6f5f-444e-a91b-658e633d5ab9-bevolking_naar_nationaliteiten_(CBS/OIS).xls
    Retrieving Geboorte naar herkomstgroepering van het kind (OIS)
    Writing to 0a4c2e95-4916-4aae-8b1b-aa389a9fc591-Geboorte_naar_herkomstgroepering_van_het_kind_(OIS).xls
    Retrieving Geboorte naar leeftijdsgroep van de moeder (OIS)
    Writing to f770f51f-f3fe-4201-b34b-40916d68c1bf-Geboorte_naar_leeftijdsgroep_van_de_moeder_(OIS).xls
    Retrieving Huishoudens naar huishoudenstypen (OIS)
    Writing to c45d3e3c-87e3-4d27-a740-6b7d916b4382-Huishoudens_naar_huishoudenstypen_(OIS).xls
    Retrieving Huishoudens naar huishoudenstypen (OIS)
    Writing to 5636ea03-44d2-4b95-99c5-2c72714a3d13-Huishoudens_naar_huishoudenstypen_(OIS).xls
    Retrieving Loop van de bevolking (OIS)
    Writing to c33811c4-cd2c-4e73-be5d-432a740535fd-Loop_van_de_bevolking_(OIS).xls
    Retrieving stand van de bevolking (CBS/OIS)
    Error while writing f145fcec-c615-4eae-bea3-bb1affc0aa23-stand_van_de_bevolking_(CBS/OIS).xls
    Retrieving woningen met dubbelglas naar bouwjaar (afd. Wonen/WIA)
    Error while writing 9d2db23e-6e31-44fd-99e0-76b9be1cef4e-woningen_met_dubbelglas_naar_bouwjaar_(afd._Wonen/WIA).xls
    Retrieving woningtoewijzingen naar woningmarktpositie (afd. Wonen/AFWC/WoningNet)
    Error while writing c52478f3-5518-4aac-a8ad-c2f351c4b295-woningtoewijzingen_naar_woningmarktpositie_(afd._Wonen/AFWC/WoningNet).xls
    Retrieving woningvoorraad en mutaties in de woningvoorraad (CBS)
    Writing to 4acd3f00-db0c-40a8-8277-002df2c09666-woningvoorraad_en_mutaties_in_de_woningvoorraad_(CBS).xls
    Retrieving arbeidsplaatsen van minimaal 12 uur per week per 1000 inwoners van 15 64 jaar naar stadsdelen (OIS)
    Writing to ff8ea36e-7222-4608-a263-011350a0f0f8-arbeidsplaatsen_van_minimaal_12_uur_per_week_per_1000_inwoners_van_15_64_jaar_naar_stadsdelen_(OIS).xls
    Retrieving werkzame personen naar buurtcombinaties (OIS/ARRA)
    Error while writing bc4bc3c3-1a55-443c-998a-894b509356b2-werkzame_personen_naar_buurtcombinaties_(OIS/ARRA).xls
    Retrieving werkzame personen naar stadsdelen (OIS/ARRA)
    Error while writing 6523416a-993b-4a8c-a426-650e44b766d2-werkzame_personen_naar_stadsdelen_(OIS/ARRA).xls
    Retrieving Bezoekers (x 1.000) aan instellingen voor beeldende kunst en expositieruimten (Instellingen)
    Writing to afdc8962-6d02-4305-bbad-044e450a86e1-Bezoekers_(x_1.000)_aan_instellingen_voor_beeldende_kunst_en_expositieruimten_(Instellingen).xls
    Retrieving Bezoekers (x 1.000) aan musea en archieven (Musea/ATCB)
    Error while writing 3926ef7f-2bcc-4b5b-9773-f78e8d3be59e-Bezoekers_(x_1.000)_aan_musea_en_archieven_(Musea/ATCB).xls
    Retrieving Bioscopen 1) en bioscoopbezoek (NVB)
    Writing to f09b2af7-0e40-4fac-966c-e743d9b6f376-Bioscopen_1)_en_bioscoopbezoek_(NVB).xls
    Retrieving Leden (OBA)
    Writing to b7b65918-7bc5-436f-b7f6-805dabcb0589-Leden_(OBA).xls
    Retrieving Uitgeleende boeken (Bibl.)
    Writing to f399b91e-aafc-448c-9949-20de67fef9c0-Uitgeleende_boeken_(Bibl.).xls
    Retrieving Uitleningen (x 1.000) van de Openbare Bibliotheek Amsterdam naar categorie (OBA)
    Writing to ae2a009b-ec8e-40ff-967f-146de51b9c43-Uitleningen_(x_1.000)_van_de_Openbare_Bibliotheek_Amsterdam_naar_categorie_(OBA).xls
    Retrieving CSV
    Writing to fa9a9007-9e2a-4043-b489-5cb7c78a26cd-CSV.xls
    Retrieving Athlon - Geaggregeerde data 2011
    Writing to e2ac3362-16f3-4158-a027-9daf6af23838-Athlon_-_Geaggregeerde_data_2011.xls
    Retrieving bedrijfsvestigingen (LISA)
    Writing to 1d5af46e-7563-43f8-9388-0e9abe2a65a3-bedrijfsvestigingen_(LISA).xls
    Retrieving bedrijfsvestigingen naar sectoren (LISA)
    Writing to 2cb0be2e-9f78-4142-b206-11a53676bb44-bedrijfsvestigingen_naar_sectoren_(LISA).xls
    Retrieving bedrijfsvestigingen naar sectoren (LISA)
    Writing to b5d5f11e-c113-4dac-915c-b6b27ab3f8a6-bedrijfsvestigingen_naar_sectoren_(LISA).xls
    Retrieving beroepsbevolking en arbeidsparticipatie (CBS)
    Writing to b95dcf64-f63e-4f55-a739-266e922ed97a-beroepsbevolking_en_arbeidsparticipatie_(CBS).xls
    Retrieving Consumentenprijsindexcijfers alle huishoudens (CBS)
    Writing to 664c7993-bb6a-4a21-b5a1-4bbc376b94fa-Consumentenprijsindexcijfers_alle_huishoudens_(CBS).xls
    Retrieving Gemeentelijke (woon)lasten (Gemeente Amsterdam/Begroting 2015)
    Error while writing 82569116-8fe3-4f9a-b16e-137a757a029d-Gemeentelijke_(woon)lasten_(Gemeente_Amsterdam/Begroting_2015).xls
    Retrieving gemeentelijke woonlasten euros per jaar per huishouden (COELO/Atlas van de lokale lasten)
    Error while writing 3419ef13-7ab4-4b85-ab7b-94b8986119bb-gemeentelijke_woonlasten_euros_per_jaar_per_huishouden_(COELO/Atlas_van_de_lokale_lasten).xls
    Retrieving gemeentelijke woonlasten euros per jaar per huishouden (COELO/Atlas van de lokale lasten)
    Error while writing 4aa4ecd5-5861-4248-87e1-7ce9817aabca-gemeentelijke_woonlasten_euros_per_jaar_per_huishouden_(COELO/Atlas_van_de_lokale_lasten).xls
    Retrieving gemiddelde huurtoeslag per jaar van huishoudens naar huishoudenstypen en leeftijdsgroepen (MinBZk/WB)
    Error while writing 220d41c3-3163-449f-a69c-104d6574f0c7-gemiddelde_huurtoeslag_per_jaar_van_huishoudens_naar_huishoudenstypen_en_leeftijdsgroepen_(MinBZk/WB).xls
    Retrieving Inkomensgegevens (CBS)
    Writing to 05e98fbc-3adc-45cf-8239-e746db06a393-Inkomensgegevens_(CBS).xls
    Retrieving Mutaties uitkeringen WW (UWV)
    Writing to 853da518-d06c-45b4-950c-d02d5e87eb7b-Mutaties_uitkeringen_WW_(UWV).xls
    Retrieving Tarieven woonlasten (Gemeente Amsterdam/Begroting 2015)
    Error while writing a0af34a6-52a3-403d-a567-c259d6ee010c-Tarieven_woonlasten_(Gemeente_Amsterdam/Begroting_2015).xls
    Retrieving Toegekende aanvragen huurtoeslag van huishoudens (MinBZk/WB)
    Error while writing 3986213f-6189-45c3-9560-27560bc8434f-Toegekende_aanvragen_huurtoeslag_van_huishoudens_(MinBZk/WB).xls
    Retrieving toegekende aanvragen huurtoeslag van huishoudens naar huishoudenstypen en leeftijdsgroepen (MinBZk/WB)
    Error while writing b010053e-d853-41fa-9fbc-ba1495d26c37-toegekende_aanvragen_huurtoeslag_van_huishoudens_naar_huishoudenstypen_en_leeftijdsgroepen_(MinBZk/WB).xls
    Retrieving Uitkeringen naar soort regeling (afd. Inkomen/UWV)
    Error while writing f5c8cca4-1f5b-4205-bbd2-0e4134afb273-Uitkeringen_naar_soort_regeling_(afd._Inkomen/UWV).xls
    Retrieving uitkeringen wajong naar mate arbeidsongeschiktheid leeftijdsgroepen en geslacht (UWV)
    Writing to 73126007-6c7b-4884-9546-df02bd4697fb-uitkeringen_wajong_naar_mate_arbeidsongeschiktheid_leeftijdsgroepen_en_geslacht_(UWV).xls
    Retrieving Uitkeringen WAO (UWV)
    Writing to a8543b5c-4e9e-4ebe-968c-b2f46104fb97-Uitkeringen_WAO_(UWV).xls
    Retrieving uitkeringen wao naar mate arbeidsongeschiktheid leeftijdsgroepen en geslacht (UWV)
    Writing to d351ae23-1e5a-4986-97a5-2eab8f8cd0e1-uitkeringen_wao_naar_mate_arbeidsongeschiktheid_leeftijdsgroepen_en_geslacht_(UWV).xls
    Retrieving uitkeringen waz naar mate arbeidsongeschiktheid leeftijdsgroepen en geslacht (UWV)
    Writing to 3eff1ddb-0fcf-4bd4-87c2-3768a23795ad-uitkeringen_waz_naar_mate_arbeidsongeschiktheid_leeftijdsgroepen_en_geslacht_(UWV).xls
    Retrieving Uitkeringen WIA (UWV)
    Writing to 40009968-7a59-4f40-b9f6-0dd3e8a1e12d-Uitkeringen_WIA_(UWV).xls
    Retrieving uitkeringen wia naar mate arbeidsongeschiktheid leeftijdsgroepen en geslacht (UWV)
    Writing to 6759363a-97f7-4ddb-bb6b-95ed905cf2f7-uitkeringen_wia_naar_mate_arbeidsongeschiktheid_leeftijdsgroepen_en_geslacht_(UWV).xls
    Retrieving uitkeringen wia wao waz en wajong (UWV)
    Writing to 9a7033a9-f663-4143-b16b-03e298916743-uitkeringen_wia_wao_waz_en_wajong_(UWV).xls
    Retrieving Uitkeringen WW (UWV)
    Writing to 0862d0f5-fcfc-459b-8c54-a4ae6e65a06d-Uitkeringen_WW_(UWV).xls
    Retrieving uitkeringen ww naar leeftijdsgroepen en geslacht (UWV)
    Writing to 0a3f0fa7-002e-4af7-95c4-fb97418fd710-uitkeringen_ww_naar_leeftijdsgroepen_en_geslacht_(UWV).xls
    Retrieving uitkeringen ww naar leeftijdsgroepen geslacht en werkloosheidspercentage (UWV)
    Writing to ca29c3ba-dc64-4394-8f41-7480227d67f0-uitkeringen_ww_naar_leeftijdsgroepen_geslacht_en_werkloosheidspercentage_(UWV).xls
    Retrieving Uitkeringsafhankelijkheid 1) naar soort regeling (afd. Inkomen/UWV)
    Error while writing 0f16e65b-4a87-4d56-8bb7-5f537a07c04f-Uitkeringsafhankelijkheid_1)_naar_soort_regeling_(afd._Inkomen/UWV).xls
    Retrieving werkzame personen (LISA)
    Writing to 6d181e60-4467-4a85-b39a-fe909bd704fd-werkzame_personen_(LISA).xls
    Retrieving werkzame personen naar sectoren (LISA)
    Writing to 207beaf9-d1e2-4002-9dd4-0c6720827c80-werkzame_personen_naar_sectoren_(LISA).xls
    Retrieving werkzame personen naar sectoren soort banen en geslacht (LISA)
    Writing to 91dd2d87-d9d5-48d9-be59-2326419ee4c5-werkzame_personen_naar_sectoren_soort_banen_en_geslacht_(LISA).xls
    Retrieving Kwaliteitswijzer (CSV)
    Writing to 37b10034-f10c-4688-b8ad-705c45d702eb-Kwaliteitswijzer_(CSV).csv
    Retrieving Kwaliteitswijzer (XLSX)
    Writing to 9011d6ff-c764-4c45-abe1-04f8f60b56fd-Kwaliteitswijzer_(XLSX).xls
    Retrieving Amsterdamse ouderen naar leeftijdsgroepen (OIS)
    Writing to a5bb294e-67b4-4ad3-985b-4b9a5c4e29d6-Amsterdamse_ouderen_naar_leeftijdsgroepen_(OIS).xls
    Retrieving sterfte naar doodsoorzaken (CBS)
    Writing to 98984452-0548-4de4-90c8-db312b7f3cd0-sterfte_naar_doodsoorzaken_(CBS).xls
    Retrieving sterfte naar doodsoorzaken (CBS)
    Writing to 541e3ef6-68fa-43d5-8acc-7a841c5dab60-sterfte_naar_doodsoorzaken_(CBS).xls
    Retrieving Algemeen
    Writing to bc8822d1-8e93-464f-9acf-7d5a62ac4366-Algemeen.zip
    Retrieving autodate
    Writing to ad2b408a-b9c5-4cfd-b925-b2a9eda9f383-autodate.zip
    Retrieving Belanghebbende
    Writing to 4cbb4804-ac09-48a7-a816-05ca63b7f379-Belanghebbende.zip
    Retrieving Invaliden algemeen
    Writing to 5cd98398-9738-46c6-a9f7-ea9cc01e475b-Invaliden_algemeen.zip
    Retrieving Invaliden kenteken
    Writing to 0cf9ea65-bc2f-4842-9681-28d8b9bd79f7-Invaliden_kenteken.zip
    Retrieving Laad-losplaatsen met locatie en tijdsbeperkingen
    Writing to 4eda3fcd-fa73-43b1-9f62-67d3a37dd39e-Laad-losplaatsen_met_locatie_en_tijdsbeperkingen.csv
    Retrieving kiesgerechtigden blancoongeldige stemmen uitgebrachte geldige stemmen en opkomstpercentage bij de verkiezingen voor de gemeenteraad 19 maart (Kiesraad/DBI/OIS)
    Error while writing b28cad0d-9627-413b-af39-643aab354545-kiesgerechtigden_blancoongeldige_stemmen_uitgebrachte_geldige_stemmen_en_opkomstpercentage_bij_de_verkiezingen_voor_de_gemeenteraad_19_maart_(Kiesraad/DBI/OIS).xls
    Retrieving kiesgerechtigden blancoongeldige stemmen uitgebrachte geldige stemmen en opkomstpercentage bij de verkiezingen voor de provinciale staten 18 maart (Kiesraad)
    Writing to 0e831b19-e7b6-4660-ba70-880f8a47df3b-kiesgerechtigden_blancoongeldige_stemmen_uitgebrachte_geldige_stemmen_en_opkomstpercentage_bij_de_verkiezingen_voor_de_provinciale_staten_18_maart_(Kiesraad).xls
    Retrieving uitgebrachte geldige stemmen blancoongeldige stemmen kiesgerechtigden en opkomstpercentage bij de verkiezingen voor de gemeenteraad 19 maart (Kiesraad/DBI/OIS)
    Error while writing bdc95875-7b24-41ba-8232-b32fe4a3d29c-uitgebrachte_geldige_stemmen_blancoongeldige_stemmen_kiesgerechtigden_en_opkomstpercentage_bij_de_verkiezingen_voor_de_gemeenteraad_19_maart_(Kiesraad/DBI/OIS).xls
    Retrieving uitgebrachte geldige stemmen blancoongeldige stemmen kiesgerechtigden en opkomstpercentage bij de verkiezingen voor de provinciale staten 18 maart (Kiesraad/DBI/OIS)
    Error while writing 69571636-d297-4d95-8775-521935beec68-uitgebrachte_geldige_stemmen_blancoongeldige_stemmen_kiesgerechtigden_en_opkomstpercentage_bij_de_verkiezingen_voor_de_provinciale_staten_18_maart_(Kiesraad/DBI/OIS).xls
    Retrieving zetelverdeling bij de verkiezingen voor de gemeenteraad 19 maart (Kiesraad/DBI/OIS)
    Error while writing bf4e76d2-18c7-452e-a69d-f9b84c242235-zetelverdeling_bij_de_verkiezingen_voor_de_gemeenteraad_19_maart_(Kiesraad/DBI/OIS).xls
    Retrieving aandeel hoogopgeleiden (Dienst Wonen, Zorg en Samenleven/Wonen in Amsterdam)
    Error while writing 847e0aa8-3091-4bb2-93e1-0e183071f330-aandeel_hoogopgeleiden_(Dienst_Wonen,_Zorg_en_Samenleven/Wonen_in_Amsterdam).xls
    Retrieving aandeel laagopgeleiden (Dienst Wonen, Zorg en Samenleven/Wonen in Amsterdam)
    Error while writing 656e8f6d-87d5-47aa-a01f-5c0ee977289a-aandeel_laagopgeleiden_(Dienst_Wonen,_Zorg_en_Samenleven/Wonen_in_Amsterdam).xls
    Retrieving basisscholen per 1000 leerlingen naar stadsdelen (OCW/DUO)
    Error while writing 4ed63330-f281-44d0-909f-979f47af9879-basisscholen_per_1000_leerlingen_naar_stadsdelen_(OCW/DUO).xls
    Retrieving gemiddelde scores eindtoets basisonderwijs cito naar stadsdelen (CITO/DMO)
    Error while writing 53456b41-429c-42ce-9279-1929bba85134-gemiddelde_scores_eindtoets_basisonderwijs_cito_naar_stadsdelen_(CITO/DMO).xls
    Retrieving Amsterdammers die een of meer keer per maand 1) sporten naar stadsdelen en leeftijdsgroepen (OIS/Sportmonitor 2013)
    Error while writing 62aa8a50-636c-4e94-a105-0d8c22688519-Amsterdammers_die_een_of_meer_keer_per_maand_1)_sporten_naar_stadsdelen_en_leeftijdsgroepen_(OIS/Sportmonitor_2013).xls
    Retrieving amsterdammers van 6 74 jaar die een of meer keer per maand sporten naar achtergrondkenmerken (OIS/Sportmonitor 2013)
    Error while writing 4106e5fa-7729-419d-8737-083eb134ee53-amsterdammers_van_6_74_jaar_die_een_of_meer_keer_per_maand_sporten_naar_achtergrondkenmerken_(OIS/Sportmonitor_2013).xls
    Retrieving bezoekers aan attractiepunten en evenementen (ATCB)
    Writing to db2b2371-0c17-4ac0-816c-bd49598cc88e-bezoekers_aan_attractiepunten_en_evenementen_(ATCB).xls
    Retrieving bezoekers aan zwembaden (Stadsdelen en zwembaden Amsterdam)
    Writing to 45dfd2a6-9d03-4a57-8e58-b49db4e528a7-bezoekers_aan_zwembaden_(Stadsdelen_en_zwembaden_Amsterdam).xls
    Retrieving Bezoekers Jaap Eden IJsbanen (Jaap Eden IJsbanen)
    Writing to f6c82e75-0800-4d0c-bddf-ccf0f91db22d-Bezoekers_Jaap_Eden_IJsbanen_(Jaap_Eden_IJsbanen).xls
    Retrieving evenementen bezoekers en exposanten in de rai hallen (RAI)
    Writing to 826f5d4b-720b-47cf-8e41-7e3ead8e45fe-evenementen_bezoekers_en_exposanten_in_de_rai_hallen_(RAI).xls
    Retrieving Lidmaatschap sportvereniging 1) naar stadsdelen en leeftijdsgroepen (OIS/Sportmonitor 2013)
    Error while writing d9f42542-b606-422d-aaec-6ff8523bb954-Lidmaatschap_sportvereniging_1)_naar_stadsdelen_en_leeftijdsgroepen_(OIS/Sportmonitor_2013).xls
    Retrieving Meest beoefende sporten (OIS/Sportmonitor 2013)
    Error while writing b26514bb-8d65-4bfc-98fd-24bbce1ba839-Meest_beoefende_sporten_(OIS/Sportmonitor_2013).xls
    Retrieving Originele data (Shapefiles en Excel)
    Writing to e1c6feb3-ebf4-4d74-8f97-92b74106cdc8-Originele_data_(Shapefiles_en_Excel).zip
    Retrieving Amsterdam
    Writing to 67440644-a8be-4559-9b0d-db47e62d262b-Amsterdam.xls
    Retrieving Centrum
    Writing to b04120ba-e82c-46df-a034-4fc4fe645b21-Centrum.xls
    Retrieving West
    Writing to ef639379-1a72-4a7c-967b-4512dd235aa5-West.xls
    Retrieving Nieuw-west
    Writing to e81c17df-ae3e-4042-a1b0-908a685cfbc6-Nieuw-west.xls
    Retrieving Zuid
    Writing to e9452665-0e07-47fe-84c4-550af4839496-Zuid.xls
    Retrieving Oost
    Writing to 4dcca1c9-b9ff-4aee-bc2c-7ed42b3c28e5-Oost.xls
    Retrieving Noord
    Writing to 289d6e40-0c16-422d-b9ee-73d317acf0ba-Noord.xls
    Retrieving Zuidoost
    Writing to 18eb8f2c-c93e-42e4-bff2-16750116c500-Zuidoost.xls
    Retrieving fosfaatuitscheiding tijdens dierlijke mestproductie (CBS)
    Writing to 2e709463-d26b-4fb6-8f12-a219774ff192-fosfaatuitscheiding_tijdens_dierlijke_mestproductie_(CBS).xls
    Retrieving geproduceerde dierlijke mest (CBS)
    Writing to 065b9769-5715-4509-878d-c93cf5260677-geproduceerde_dierlijke_mest_(CBS).xls
    Retrieving kali uitscheiding tijdens dierlijke mestproductie (CBS)
    Writing to 1496eaf3-17b8-4568-b0a0-fb5bf6497b5c-kali_uitscheiding_tijdens_dierlijke_mestproductie_(CBS).xls
    Retrieving meldingen met betrekking tot geluidshinder schiphol gebruiksjaar (Bewoners Aanspreekpunt Schiphol)
    Writing to a7bafee7-910b-4071-ba29-054a40e29c14-meldingen_met_betrekking_tot_geluidshinder_schiphol_gebruiksjaar_(Bewoners_Aanspreekpunt_Schiphol).xls
    Retrieving stikstofproductie tijdens dierlijke mestproductie (CBS)
    Writing to 7d047e10-6b27-4b71-9f32-b6837ddf2b95-stikstofproductie_tijdens_dierlijke_mestproductie_(CBS).xls
    Retrieving aanvoer en afvoer van klein chemisch afval (AEB)
    Writing to 0500b53f-0d53-4be0-948f-0111ed9f9b1a-aanvoer_en_afvoer_van_klein_chemisch_afval_(AEB).xls
    Retrieving aanvoerstromen van afval (AEB)
    Writing to 59fcb1bf-1ee3-482b-ac6e-7393b5e08a5e-aanvoerstromen_van_afval_(AEB).xls
    Retrieving geproduceerde dierlijke mestsoorten (CBS)
    Writing to ac453703-e9c5-432e-882d-7e9ec66a217f-geproduceerde_dierlijke_mestsoorten_(CBS).xls
    Retrieving Huishoudelijk afval (kg/inwoner) (CBS)
    Error while writing b79e4130-89f3-47ff-87e6-6f643e8daa07-Huishoudelijk_afval_(kg/inwoner)_(CBS).xls
    Retrieving uitgescheiden mineralen tijdens dierlijke mestproductie (CBS)
    Writing to 76daae68-1172-4c46-8959-1f1421ad0860-uitgescheiden_mineralen_tijdens_dierlijke_mestproductie_(CBS).xls
    Retrieving waterbronnen en levering (Waternet)
    Writing to 719dd0ea-daaa-4115-9c66-f4b183b32c32-waterbronnen_en_levering_(Waternet).xls
    Retrieving waterlevering naar verbruiksgroepen (Waternet)
    Writing to ff12804e-324d-44bc-8120-db7bbe2176a0-waterlevering_naar_verbruiksgroepen_(Waternet).xls
    Retrieving CSV-bestand
    Writing to ac6a936f-eaaa-4f78-8208-59e4310493c3-CSV-bestand.xls
    Retrieving gemeentelijke woonlasten euros per jaar per huishouden (COELO/Atlas van de sociale lasten)
    Error while writing e7205b28-f4b0-44a8-9f64-6dcee980e812-gemeentelijke_woonlasten_euros_per_jaar_per_huishouden_(COELO/Atlas_van_de_sociale_lasten).xls
    Retrieving gemiddelde huurtoeslag per jaar van huishoudens naar huishoudenstypen en leeftijdsgroepen (MinBZk/WB)
    Error while writing d0f32c91-b012-4283-bc6b-2cd38152ec9e-gemiddelde_huurtoeslag_per_jaar_van_huishoudens_naar_huishoudenstypen_en_leeftijdsgroepen_(MinBZk/WB).xls
    Retrieving particuliere huishoudens incl studenten naar inkomensklassen (CBS/RIO 2012)
    Error while writing 89d62d27-85c6-4386-84f4-2aaa72e2464d-particuliere_huishoudens_incl_studenten_naar_inkomensklassen_(CBS/RIO_2012).xls
    Retrieving toegekende aanvragen huurtoeslag van huishoudens naar huishoudenstypen en leeftijdsgroepen (MinBZk/WB)
    Error while writing 7d963430-1eed-43f6-aba0-81a3e92b1c40-toegekende_aanvragen_huurtoeslag_van_huishoudens_naar_huishoudenstypen_en_leeftijdsgroepen_(MinBZk/WB).xls
    Retrieving uitkeringen wajong (UWV)
    Writing to 462a0a73-d165-449e-9910-f717abce4ddd-uitkeringen_wajong_(UWV).xls
    Retrieving uitkeringen wajong naar mate arbeidsongeschiktheid leeftijdsgroepen en geslacht (UWV)
    Writing to 4d1ae070-22de-469c-95e0-8ee5432615e7-uitkeringen_wajong_naar_mate_arbeidsongeschiktheid_leeftijdsgroepen_en_geslacht_(UWV).xls
    Retrieving uitkeringen wao (UWV)
    Writing to 1a19d270-83df-494d-8697-808c16680a29-uitkeringen_wao_(UWV).xls
    Retrieving uitkeringen wao naar mate arbeidsongeschiktheid leeftijdsgroepen en geslacht (UWV)
    Writing to 83a61b27-930b-4802-bed1-20b752d81820-uitkeringen_wao_naar_mate_arbeidsongeschiktheid_leeftijdsgroepen_en_geslacht_(UWV).xls
    Retrieving uitkeringen waz (UWV)
    Writing to 04b932fa-7d5d-40a1-a978-312d8cbccfda-uitkeringen_waz_(UWV).xls
    Retrieving uitkeringen waz naar mate arbeidsongeschiktheid leeftijdsgroepen en geslacht (UWV)
    Writing to d9ba4fba-9894-4ea5-93a8-3a9d92109e45-uitkeringen_waz_naar_mate_arbeidsongeschiktheid_leeftijdsgroepen_en_geslacht_(UWV).xls
    Retrieving uitkeringen wia (UWV)
    Writing to ea8b23f6-5a10-4738-9932-a7330349bf12-uitkeringen_wia_(UWV).xls
    Retrieving uitkeringen wia naar mate arbeidsongeschiktheid leeftijdsgroepen en geslacht (UWV)
    Writing to 10cdbb45-f9b5-4bf5-89c2-0ce342b4164f-uitkeringen_wia_naar_mate_arbeidsongeschiktheid_leeftijdsgroepen_en_geslacht_(UWV).xls
    Retrieving uitkeringen ww (UWV)
    Writing to fa787c1b-b583-41d8-acf5-0209f7ca1302-uitkeringen_ww_(UWV).xls
    Retrieving uitkeringen ww naar leeftijdsgroepen geslacht en werkloosheidspercentage (UWV)
    Writing to 0001c640-8726-4fb8-ad6c-c7158289010d-uitkeringen_ww_naar_leeftijdsgroepen_geslacht_en_werkloosheidspercentage_(UWV).xls
    Retrieving Bevolking
    Writing to b86868a9-3eb6-461a-a9e6-1938bd2fdf39-Bevolking.xls
    Retrieving Bevolking naar leeftijdsgroepen
    Writing to f0115db4-d27a-4a23-88ba-f716b137d173-Bevolking_naar_leeftijdsgroepen.xls
    Retrieving PotentiÃ«le beroepsbevolking naar leeftijdsgroepen
    Writing to 9115b298-8cd3-40e8-a010-f68d866525a9-PotentiÃ«le_beroepsbevolking_naar_leeftijdsgroepen.xls
    Retrieving Bevolking naar herkomstgroepering
    Writing to 51f666c8-9f08-450d-9e7b-6e2d7d7db821-Bevolking_naar_herkomstgroepering.xls
    Retrieving Huishoudens naar huishoudenstypen
    Writing to 03242bb1-cc12-497f-8bed-b4aca5e0ebbf-Huishoudens_naar_huishoudenstypen.xls
    Retrieving kiesgerechtigden blancoongeldige stemmen uitgebrachte geldige stemmen en opkomstpercentage bij de verkiezingen voor de gemeenteraad 19 maart (Kiesraad/DBI/OIS)
    Error while writing a25abe00-e142-446a-952c-acaa6f4473f5-kiesgerechtigden_blancoongeldige_stemmen_uitgebrachte_geldige_stemmen_en_opkomstpercentage_bij_de_verkiezingen_voor_de_gemeenteraad_19_maart_(Kiesraad/DBI/OIS).xls
    Retrieving kiesgerechtigden blancoongeldige stemmen uitgebrachte geldige stemmen en opkomstpercentage bij de verkiezingen voor de provinciale staten 18 maart (Kiesraad)
    Writing to f664546f-a6a8-455e-a6ce-b987839f4b7e-kiesgerechtigden_blancoongeldige_stemmen_uitgebrachte_geldige_stemmen_en_opkomstpercentage_bij_de_verkiezingen_voor_de_provinciale_staten_18_maart_(Kiesraad).xls
    Retrieving CSV
    Writing to 36e4886f-136e-4957-b8ef-b1995bb7062f-CSV.xls
    Retrieving Vloeroppervlakte (x 1.000 m²) naar buurtcombinaties en soort gebruik, 1 januari 2010
    Writing to 7d71ce8e-3241-4b09-8f9c-efb522020741-Vloeroppervlakte_(x_1.000_m²)_naar_buurtcombinaties_en_soort_gebruik,_1_januari_2010.xls
    Retrieving Vloeroppervlakte (x 1.000 m²) naar stadsdelen en soort gebruik, 1 januari 2012
    Writing to 5fd02964-43f7-4531-ab60-435faf829e7e-Vloeroppervlakte_(x_1.000_m²)_naar_stadsdelen_en_soort_gebruik,_1_januari_2012.xls
    Retrieving Excel
    Writing to 3685ac00-7144-402b-bab5-ef87ccf7b64e-Excel.xls
    Retrieving Excel
    Writing to 9b4290a1-0952-43a6-8ad3-8bc10c77abc1-Excel.xls
    Retrieving Excel (via O S)
    Writing to 6b67fc74-11db-454d-923f-fc66ab438436-Excel_(via_O_S).xls
    Retrieving Projecten
    Writing to aec3148a-8bd8-43ca-b079-94a02759501a-Projecten.json
    Retrieving Locaties (JSON)
    Writing to 615b8996-c414-4dcc-ac0d-3ad45550b126-Locaties_(JSON).json
    Retrieving P R Routes (JSON)
    Writing to e11630fd-b5aa-4a0f-b435-f7b58893d3b8-P_R_Routes_(JSON).json
    Retrieving Aanbevolen route voor touringcars (JSON)
    Writing to feb70244-eead-458b-b080-8246380dd9ec-Aanbevolen_route_voor_touringcars_(JSON).json
    Retrieving In- en uitstaphaltes voor touringcars in Amsterdam (JSON)
    Writing to badf456f-71ab-4d96-a958-96916e587de3-In-_en_uitstaphaltes_voor_touringcars_in_Amsterdam_(JSON).json
    Retrieving Maximale doorrijhoogtes (viaducten etc) (JSON)
    
    Writing to 2a790ef3-7df2-40ec-beda-966e1a1e3ed1-Maximale_doorrijhoogtes_(viaducten_etc)_(JSON)
    .json
    Retrieving Parkeerplaatsen voor touringcars (JSON)
    Writing to 2e116ed1-5185-420e-8351-f0dcc01c9572-Parkeerplaatsen_voor_touringcars_(JSON).json
    Retrieving Locaties/weggedeeltes met tijdelijke verkeershinder voor touringcars (JSON)
    
    Error while writing 8121504c-f650-4019-af1f-2f5de550b80b-Locaties/weggedeeltes_met_tijdelijke_verkeershinder_voor_touringcars_(JSON)
    .json
    Retrieving Verplichte route touringcars (JSON)
    
    Writing to bc6c3f95-f5a8-4bce-b7a8-5bff1852cc20-Verplichte_route_touringcars_(JSON)
    .json
    Retrieving Data (JSON)
    Writing to 9a55c876-8198-46b1-862e-d2b059505fc8-Data_(JSON).json
    Retrieving Locaties (JSON)
    Writing to 47e80274-a347-4f6d-b7d7-e67e1a1a2dfb-Locaties_(JSON).json
    Retrieving Locaties (JSON)
    Writing to cc2f98a9-b6bf-4409-945c-a689216688be-Locaties_(JSON).json
    Retrieving Rooutes gevaarlijke stoffen (JSON)
    Writing to be01d6a2-d146-4a3f-a2ac-d99e8ceac2cd-Rooutes_gevaarlijke_stoffen_(JSON).json
    Retrieving Tunnels gevaarlijke stoffen (JSON)
    Writing to d0ea393b-b813-41b3-9e93-0d21f28fe584-Tunnels_gevaarlijke_stoffen_(JSON).json
    Retrieving Kwaliteitsnet Noordvleugel vrachtverkeer (JSON)
    Writing to 54a839d1-ff6a-4111-a9d9-a9558859c4e0-Kwaliteitsnet_Noordvleugel_vrachtverkeer_(JSON).json
    Retrieving Verplichte routes zwaar vrachtverkeer in Centrum (JSON)
    Writing to 5a3af674-0d02-45dc-a8f7-1e68b1437a48-Verplichte_routes_zwaar_vrachtverkeer_in_Centrum_(JSON).json
    Retrieving Voorkeursnetwerk vrachtverkeer (JSON)
    Writing to abb22487-2bde-4287-9a77-f29d6f31fc43-Voorkeursnetwerk_vrachtverkeer_(JSON).json
    Retrieving Uit in Amsterdam (CSV)
    Writing to 02d34648-7583-405a-ad9e-78c82c923299-Uit_in_Amsterdam_(CSV).csv
    Retrieving Uit in Amsterdam (JSON)
    Writing to 09c9c862-f346-4331-9956-dcaedf5d1be4-Uit_in_Amsterdam_(JSON).json
    Retrieving Evenementen (CSV)
    Writing to fbb649c7-73bb-47e3-8e3d-05d7fd1dc87a-Evenementen_(CSV).csv
    Retrieving Evenementen (JSON)
    Writing to 68b0abe7-da12-425f-96d6-07b1910360fa-Evenementen_(JSON).json
    Retrieving Locaties (JSON)
    Writing to 0a064f5a-8cdd-4c3c-aaa6-ff612190cd11-Locaties_(JSON).json
    Retrieving Winkels (CSV)
    Writing to 82057d4f-33b8-45e9-9677-b2f433341eb3-Winkels_(CSV).csv
    Retrieving Winkels (JSON)
    Writing to bce63923-a3a6-4157-aed1-a810cd036da0-Winkels_(JSON).json
    Retrieving Theater (CSV)
    Writing to d2be76dc-4588-41b3-a350-20dd27dd66d6-Theater_(CSV).csv
    Retrieving Theater (JSON)
    Writing to 36ca3eb3-f0e8-40d5-bee2-3283d7e6fb68-Theater_(JSON).json
    Retrieving Musea & Galleries (CSV)
    Writing to 82a4b54a-d3df-4bc7-b345-4d51c86d1290-Musea_&_Galleries_(CSV).csv
    Retrieving Musea & Galleries (JSON)
    Writing to 0336103b-db4e-454b-b9f1-11691fbac5d5-Musea_&_Galleries_(JSON).json
    Retrieving Tentoonstellingen (CSV)
    Writing to 8a84455f-b889-4f7d-b919-963c2dc2228d-Tentoonstellingen_(CSV).csv
    Retrieving Tentoonstellingen (JSON)
    Writing to bf8b91a3-17f5-44b6-9bfd-d9cdd1c20013-Tentoonstellingen_(JSON).json
    Retrieving Attracties en bezienswaardigheden (CSV)
    Writing to 12ae90de-3b7f-4de4-b6c4-4c63627f379a-Attracties_en_bezienswaardigheden_(CSV).csv
    Retrieving Attracties en bezienswaardigheden (JSON)
    Writing to e5a34d9a-70f6-4cc1-9a19-c850f61ca4aa-Attracties_en_bezienswaardigheden_(JSON).json
    Retrieving Activiteiten (CSV)
    Writing to 5c04fd6b-c1ff-4141-b158-298b4c2952ae-Activiteiten_(CSV).csv
    Retrieving Activiteiten.json
    Writing to 1df28140-8e59-42b7-baf2-1fb7a88cff32-Activiteiten.json.json
    Retrieving Eten & Drinken (CSV)
    Writing to 6556694a-2241-4a9c-b20b-be8e2d0e0a2b-Eten_&_Drinken_(CSV).csv
    Retrieving Eten & Drinken (JSON)
    Writing to 3cf26023-c688-4007-929e-c784e0397b7f-Eten_&_Drinken_(JSON).json
    Retrieving Festivals (CSV)
    Writing to d55d2cf4-0c03-4f70-b8ca-046b5ee79c35-Festivals_(CSV).csv
    Retrieving Festivals (JSON)
    Writing to e9de1aed-1fe2-4f65-af07-58d8477ac567-Festivals_(JSON).json
    Retrieving Metadata en voorbeeldcode
    Writing to e9d0a508-db9a-4a1f-ad2f-c1a6044dbab0-Metadata_en_voorbeeldcode.zip
    Retrieving Historie reistijden januari 2014
    Writing to 84228d75-e8e0-4091-9d6d-5443b31e3b9d-Historie_reistijden_januari_2014.zip
    Retrieving Lijst van alle bronnen en aantallen
    Writing to f56ebf7d-79cf-4056-b22a-8a2e9d7092b8-Lijst_van_alle_bronnen_en_aantallen.api
    Retrieving BAG (WFS)
    Writing to b37f3547-be9a-4029-afff-29316b59b44b-BAG_(WFS).api
    Retrieving API via CitySDK (voorbeeldlink)
    Writing to 800038f9-9e31-40a9-8934-04f54d020999-API_via_CitySDK_(voorbeeldlink).api
    Retrieving BAG Ligplaatsen (via CitySDK)
    Writing to 3f4451e6-30ab-45c8-ac36-e545acb31d4f-BAG_Ligplaatsen_(via_CitySDK).json
    Retrieving BAG Panden (via CitySDK)
    Writing to 640b06bf-fc15-4561-b4a0-25a3f39b0b51-BAG_Panden_(via_CitySDK).json
    Retrieving Bag VBO (via CitySDK)
    Writing to 638fca12-7fed-408b-b0f1-2031cbd8da03-Bag_VBO_(via_CitySDK).json
    Retrieving BAG Standplaatsen (via CitySDK)
    Writing to 23e82484-9be8-4052-8f52-045d81303d2f-BAG_Standplaatsen_(via_CitySDK).json
    Retrieving API
    Writing to 82e9b535-10f8-4d6b-bac0-c2b1aea398b9-API.api
    Retrieving API + toelichting
    Writing to 1a0881ef-dff4-40fc-af5e-7bfaf021d5a8-API_+_toelichting.api
    Retrieving Open Search API
    Writing to 02849b08-7a3e-43c9-8ec2-b7728be9c121-Open_Search_API.api
    Retrieving API (Statische info)
    Writing to 4f8dbbed-290f-4395-819d-5ab96e99b788-API_(Statische_info).api
    Retrieving API (Dynamische info)
    Writing to b5a569f4-622d-4570-ad6c-3ff14f0aef1a-API_(Dynamische_info).api
    Retrieving Dynamische gegevens (API)
    Writing to 9253c30e-e5f9-4ac2-b3a7-1f6240ae9372-Dynamische_gegevens_(API).api
    Retrieving API   documentatie
    Writing to e89f3da7-01f7-4113-8f6d-d14736e64d72-API___documentatie.api
    Retrieving Afvalbakken (Shapefile in RD)
    Writing to fe78e71e-96d4-4e4d-a17d-94d6f99975e5-Afvalbakken_(Shapefile_in_RD).zip
    Retrieving Afvalbakken (Shapefile in WGS84)
    Writing to 83ee76af-4997-4ffc-936c-946cb744582c-Afvalbakken_(Shapefile_in_WGS84).zip
    Retrieving afvalbakken met geocoordinaten in csv
    Writing to 6eed5607-4231-411f-9817-4d2fbc292812-afvalbakken_met_geocoordinaten_in_csv.csv
    Retrieving API (PP4)
    Writing to 67d26fcd-642a-43ff-ab7f-6290be559364-API_(PP4).api
    Retrieving API (PP5)
    Writing to d363caa2-379c-46ea-86ec-9d208296f013-API_(PP5).api
    Retrieving API (PP6)
    Writing to 6a1bd310-5396-4570-896a-f818b796a919-API_(PP6).api
    Retrieving Overzicht APIs en toelichting
    Writing to 37c8f8f1-9d5a-41b1-9671-636d6dd02102-Overzicht_APIs_en_toelichting.api
    Retrieving API
    Writing to 23640c48-2610-4416-b7ee-e51ce0760cfb-API.api
    Retrieving API
    Writing to 8fe28611-9e8d-4306-8730-ebb9fb49be2c-API.api
    Retrieving Kleinverbruiksgegevens 1 jan. 2016
    Writing to e7d626e5-99d3-45bd-97ee-338dc5984347-Kleinverbruiksgegevens_1_jan._2016.zip
    Retrieving API
    Writing to 54fdb74e-b1e7-4f7d-8a0e-1d1420274175-API.api
    Retrieving API (SOAP/XML)
    Error while writing bbbcbe38-acbd-45be-957c-0a54eee0108e-API_(SOAP/XML).api
    Retrieving Brandmeldingen 2010-2015
    Writing to bcd89a9e-b249-4939-ae0a-0e7a4bac1a08-Brandmeldingen_2010-2015.csv
    Retrieving Data
    Writing to d4dc66b0-61c3-42ab-9573-3151df39d2ec-Data.zip
    Retrieving NDOV Loket (API)
    Writing to 61392167-426b-4a36-96b9-784c380dc792-NDOV_Loket_(API).api
    Retrieving Haltes (voorbeeld via CitySDK)
    Writing to f802c39e-aa78-4e82-b9ce-13169b4c09f6-Haltes_(voorbeeld_via_CitySDK).json
    Retrieving Actuele vertragingen (voorbeeld via CitySDK)
    Writing to 8d439330-63e5-4ef2-8e06-b9ad2c5b7763-Actuele_vertragingen_(voorbeeld_via_CitySDK).json
    Retrieving Basisbestand Gebieden Amsterdam
    Writing to 6706ae42-1f15-4e43-b0d4-c9f3f525620e-Basisbestand_Gebieden_Amsterdam.xls
    Retrieving Hotels MRA 2014
    Writing to ca0becd8-30c7-492b-8dd2-8c0d2a4c5637-Hotels_MRA_2014.xls
    Retrieving Hotels MRA 2012
    Writing to 8e15e081-2ca1-4970-aecd-3f07da158321-Hotels_MRA_2012.csv
    Retrieving Alle datasets en resource
    Writing to 41436992-0976-4989-8058-9f856f62ef47-Alle_datasets_en_resource.xls
    Retrieving Documentatie API
    Writing to 2e229ce6-64d8-4cf8-b496-1dcb30d9c72f-Documentatie_API.api
    Retrieving API
    Writing to e0df00bf-cfbe-4558-bb13-e99eb3ee719f-API.json
    Retrieving Complete lijst met datasets via API
    Writing to 73fbf10f-275c-414f-a4cd-c488348789ff-Complete_lijst_met_datasets_via_API.json
    Retrieving Stadsdeel Oost
    Writing to 19c2797e-6970-483c-9637-a5ece50fe77b-Stadsdeel_Oost.xls
    Retrieving Stadsdeel Zuid
    Writing to df49d18c-34fa-4b94-93e8-76472105f016-Stadsdeel_Zuid.xls
    Retrieving Rapport, data en geografie
    Writing to ab006f1f-8a2c-4067-a44c-74559f774120-Rapport,_data_en_geografie.zip
    Retrieving Fietsnetwerk
    Writing to 90ba4399-7c2b-4e2e-a36a-7a26f24f2dbf-Fietsnetwerk.zip



    
